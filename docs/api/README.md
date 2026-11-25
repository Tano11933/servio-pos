# Kontrak REST awal

Status: rancangan interface, bukan endpoint yang sudah tersedia atau OpenAPI yang sudah dibangkitkan. Mengacu [PRD](../product/prd.md), [model data](../architecture/data-model.md), dan [ADR transaksi](../architecture/adr/002-transactions-and-recovery.md). Schema request/response rinci untuk katalog, inventori dan laporan diselesaikan sebelum fitur terkait diimplementasikan.

## Konvensi

Base `/api/v1`. Resource operasional diawali `/tenants/{tenantId}/outlets/{outletId}` (disingkat `{scope}` di tabel). Session cookie, CSRF token pada mutasi, dan membership/outlet berlaku pada semua operasi kecuali login/health. Role mengikuti matriks akses yang masih usulan; URL scope bukan bukti izin.

ID UUID string; uang string digit (`"41580"`), quantity bahan string desimal, rate string persen, kuantitas menu JSON integer positif. Waktu ISO 8601 UTC; response settlement menyertakan currency dan timezone snapshot. Field writable di-allowlist; tenant/actor/source harga tidak diambil dari body.

Response sukses `{ "data": ..., "requestId": "..." }`. Daftar memakai `{ "data": [], "nextCursor": null, "requestId": "..." }`; cursor opaque terikat filter/scope, urutan created_at/id stabil, limit default 25 maksimum 100. Laporan memakai rentang UTC hasil konversi tanggal outlet [awal,akhir); timezone tidak ditebak dari browser.

Error `{ "error": { "code": "VERSION_CONFLICT", "message": "...", "retryable": false }, "requestId": "..." }`. Field error aman boleh menyertakan nama field dan versi terkini untuk resource yang sudah diotorisasi, tanpa data tenant lain.

| HTTP | Code utama | Makna |
| --- | --- | --- |
| 400 | INVALID_REQUEST | Tipe/format/unknown field tidak valid |
| 401 | UNAUTHENTICATED | Sesi tidak ada/kedaluwarsa/dicabut |
| 403 | FORBIDDEN, CSRF_FAILED | Scope/izin atau proteksi mutasi gagal |
| 404 | NOT_FOUND | Resource tidak ada; ID di luar scope diperlakukan tidak ada |
| 409 | VERSION_CONFLICT, IDEMPOTENCY_CONFLICT, STATE_CONFLICT, INSUFFICIENT_STOCK | Muat ulang/perbaiki prasyarat; jangan retry otomatis dengan key baru |
| 422 | BUSINESS_RULE_VIOLATION | Nilai melampaui hak refund, kas tidak cukup, recipe/config tidak lengkap |
| 429 | RATE_LIMITED | Tunggu sesuai Retry-After |
| 503 | RETRYABLE_TRANSACTION, OPERATION_UNCERTAIN | Lihat retryable; hasil ambigu wajib lookup/retry key sama |

Mutasi operasional memakai header `Idempotency-Key` UUID dan body `expectedVersion` untuk agregat yang sudah ada. Membuat draft/shift baru tidak punya expectedVersion; unique constraint melindungi meja/shift aktif. Perintah yang memengaruhi beberapa agregat mengunci semuanya, tetapi expectedVersion klien merujuk agregat utama pada path. Versi shift diverifikasi terkini di bawah lock. Semua respons sukses idempotent menyimpan hasil yang akan direplay. Konfirmasi payload yang berubah membuat operasi baru dengan key baru hanya setelah hasil operasi sebelumnya diketahui.

## Operasi prioritas

Path berikut adalah kontrak awal. READ tanpa idempotency key; mutasi bisnis memakai aturan di atas. Izin disebut sesuai usulan matriks, bukan persetujuan baru.

| Method/path | Izin/prasyarat | Input utama → hasil |
| --- | --- | --- |
| POST `/auth/login` | Kredensial; rate limit | login/password → sesi cookie baru; tidak menerima tenant bebas sebagai otorisasi |
| POST `/auth/logout` | Sesi+CSRF | Cabut sesi → status logout; aman diulang |
| GET `/auth/me` | Sesi | User, memberships, assignments, CSRF token sesi; tidak dicache publik |
| GET `{scope}/menu` | Katalog kasir | cursor/filter → menu/varian/modifier/ketersediaan/version |
| POST `{scope}/orders` | Order write | mode, tableId bila dine-in → draft ID/version; race meja menghasilkan conflict |
| PATCH `{scope}/orders/{id}/draft-items` | Order write; belum dibayar | expectedVersion, daftar edit draft, menu/variant/modifier IDs dan qty → draft terbaru dengan quote version |
| POST `{scope}/orders/{id}/batches` | Order write | expectedVersion, draftItemIds, confirmedQuoteVersion → batch/item snapshot dan order version; perubahan quote perlu konfirmasi ulang |
| GET `{scope}/orders/{id}` | Akses operasional | Snapshot order dan status yang boleh dilihat aktor |
| GET `{scope}/kitchen/queue` | Kitchen read | Snapshot seluruh item aktif/dibatalkan terbaru sesuai jendela/filter; polling bukan event stream |
| POST `{scope}/orders/{id}/items/{itemId}/start` | Kitchen write; QUEUED | expectedVersion order → PREPARING dan versi baru; konsumsi hanya qty aktif |
| POST `{scope}/orders/{id}/items/{itemId}/ready` | Kitchen write; PREPARING | expectedVersion order → READY; order dapat CLOSED bila dibayar dan semua item final |
| POST `{scope}/orders/{id}/cancellations` | Draft: pembuat/peran berizin; terkirim: owner/manager; belum dibayar | expectedVersion, itemIds atau wholeOrder, reason → hasil pembatalan; pilihan keduanya sekaligus ditolak |
| POST `{scope}/orders/{id}/payments` | Settlement write; shift sendiri OPEN | expectedVersion, shiftId, method, received?, confirmedTotal, manualConfirmed → payment/receipt/order; QRIS/EDC wajib konfirmasi |
| POST `{scope}/payments/{id}/refunds` | Owner/manager; shift sendiri OPEN | expectedVersion order, shiftId, lines item/qty/disposition, reason, manualConfirmed, reference? → refund; amount dihitung server |
| GET `{scope}/orders/{id}/receipt` | Struk operasional | Snapshot struk; tidak menulis penjualan atau menjamin printer berhasil |
| POST `{scope}/shifts` | Peran berizin | openingCash → shift sendiri OPEN |
| POST `{scope}/shifts/{id}/cash-movements` | Shift sendiri OPEN | expectedVersion shift, direction, amount, reason → movement dan shift version |
| POST `{scope}/shifts/{id}/close` | Shift sendiri OPEN | expectedVersion shift, actualCash, discrepancyReason? → snapshot expected/actual/difference |
| GET `{scope}/operations/{key}` | Aktor pemilik key, izin terkini | Hasil tersimpan, resource dan HTTP outcome; 404 bukan bukti operasi lain tidak sedang berjalan |
| GET `{scope}/reports/sales` | Owner/manager | from/to, cursor → sumber/sum sesuai snapshot; schema rinci sebelum laporan |
| GET `/health/live`, `/health/ready` | Infrastruktur; tanpa detail sensitif | Liveness proses, readiness DB; tidak mengungkap secret |

Untuk refund path payment, `expectedVersion` adalah versi order yang diperoleh lewat payment/order detail; kontrak ini harus ditampilkan jelas di OpenAPI. Semua start/ready memakai versi order, bukan versi item, sehingga perubahan tagihan paralel menghasilkan konflik eksplisit yang dipulihkan klien.

## Contoh pelunasan

```json
{
  "expectedVersion": 4,
  "shiftId": "7cf52de8-151a-4d5b-907b-f6e971207dd7",
  "method": "CASH",
  "received": "50000",
  "confirmedTotal": "41580",
  "manualConfirmed": true
}
```

Header Idempotency-Key berbeda untuk tindakan baru dan tetap sama saat retry. Server membaca snapshot, bukan mempercayai confirmedTotal sebagai harga. Contoh response data: paymentId/orderId, method, currency, total `"41580"`, received `"50000"`, change `"8420"`, orderVersion dan paymentStatus. Field received hanya untuk CASH; pada metode digital nilainya diturunkan dari total, bukan amount bebas.

## Dapur dan data sensitif

Queue menyertakan order/item/batch IDs, version order terkini, meja/mode, catatan, kuantitas aktif dan status; tidak mengirim nominal pembayaran/hak pengguna/laporan. Saat cursor/history terbatas, snapshot aktif tetap lengkap; item yang tidak lagi aktif dipulihkan lewat refresh order/status. Ekspor memiliki otorisasi serta batas ukuran sendiri dan tidak memakai response API yang bisa dicache service worker.

Sebelum implementasi, lengkapi JSON Schema/OpenAPI request/response, enum, batas field/range, dan pemetaan permission per operasi. Nama endpoint ini cukup untuk desain alur, bukan klaim seluruh wire schema sudah final. Unknown outcomes tidak boleh ditampilkan sebagai gagal bayar pasti.
