# Strategi pengujian arsitektur

Status: rancangan. Belum ada aplikasi, schema, database uji atau test runner di repository. Pemeriksaan dokumen tidak sama dengan pengujian runtime.

## Tingkatan bukti

| Lapisan | Fokus | Bukti minimum |
| --- | --- | --- |
| Unit domain | Uang/rational, alokasi refund, konversi, transisi | Contoh BR/EX dan batas angka menghasilkan nilai eksak; tidak hanya menyalin implementasi |
| PostgreSQL integration | Composite FK, check, partial unique, transaksi | Dua tenant/outlet, migrasi dari nol, rollback tanpa ledger/audit/operation parsial |
| API integration | Sesi/CSRF/izin, body allowlist, scope, error | Request langsung di luar UI ditolak; revoke berlaku pada request berikutnya |
| Konkurensi | Lock order, idempotency, stok, meja, shift, refund | Dua koneksi dengan barrier terkoordinasi, bukan delay acak atau mock |
| Browser E2E | Kasir, dapur, konflik, polling, draft | Takeaway dan dine-in penuh; keyboard, sentuhan, loading/error/reconnect |
| Operasional | Printer, deployment, restore, beban | Perangkat fisik/staging dan hasil terukur; konfigurasi target dicatat |

Gunakan fixture terisolasi tanpa data pelanggan dan DB khusus uji. Pengujian database tidak boleh diarahkan ke DB produksi; verifikasi target environment sebelum migrasi/reset data uji. Framework/perintah baru ditambahkan setelah scaffold dapat dijalankan.

## Review invariant sebelum merge kode

1. FK komposit menolak relasi lintas tenant/outlet, termasuk refund/payment/shift dan ingredient snapshot.
2. Dua pelunasan berbeda untuk order sama menghasilkan satu payment dan satu entry kas. Retry key sama mengembalikan hasil identik; key sama payload berbeda ditolak.
3. Dua batch memperebutkan bahan terakhir: satu kalah utuh; tidak ada reservasi sebagian.
4. Start vs refund QUEUED dan cancel vs start berserialisasi; qty konsumsi/reservasi/waste cocok tanpa pengurangan ganda.
5. Close shift vs payment/refund mengurutkan operasi: expected cash tetap bisa direkonsiliasi dan CLOSED tidak berubah.
6. Dua pembukaan meja/shift aktif dan dua refund unit terakhir terkena constraint yang benar.
7. Revoke session/membership bersamaan dengan command memiliki urutan sebagaimana ADR; replay setelah revoke tidak membuka data.
8. Restore dan hasil COMMIT ambigu tidak memicu key baru atau pengiriman uang eksternal berulang.
9. Kuantitas 6 desimal, uang di atas batas safe integer JavaScript, overflow bigint dan rate batas divalidasi tanpa precision loss.
10. Alokasi tiap unit menjumlah tepat gross/discount/net/service/tax/total dan refund akhir; nol bobot basis tidak membagi dengan nol (komponen nol dialokasikan nol).

## Fault injection

Putus sebelum commit → seluruh efek dan Operation rollback. Putus sesudah commit sebelum response → lookup menemukan hasil. Lock timeout → respons belum pasti di klien, retry key sama. Kill proses lalu mulai ulang → outcome persisten tetap ada. Retry deadlock terbatas dan tidak menjalankan efek eksternal di dalam transaksi.

## Keterlacakan

- ACC-01/02/03 → API scope/session/CSRF/revoke dan integration FK.
- CAT-01/02 → validasi katalog, varian/modifier, menu nonaktif, manipulasi harga klien, dan pembaruan versi saat konfirmasi quote sebelum kirim.
- INV-01/02/03, ORD-01/02/03/04, KIT-01 → snapshot, stock barrier, queue dan alur E2E.
- PAY-01/02/03/04, REC-01, SHF-01/02, CAN-01, RET-01, AUD-01 → uang, transaksi/refund, close race, cetak serta audit.
- REP-01/02 → rekonsiliasi sumber/periode, tenant/outlet filter, export bounds.
- UX-01, RES-01/02, PRT-01 → keyboard/Android/PWA/draft/printer fisik.
- OPS-01/02/03 → deploy/health/log, restore dengan RPO/RTO, dan beban target pilot.

Skenario rinci tetap berada pada [matriks validasi](../product/acceptance-matrix.md) dan [contoh bisnis](../product/business-examples.md). Persetujuan arsitektur tidak berarti kasus tersebut sudah dijalankan.

## Review dokumentasi saat ini

Periksa tautan lokal, urutan heading/fence Mermaid, JSON contoh valid, kesesuaian field/path API dengan model, pemetaan requirement, whitespace, label dan metadata commit. Mermaid adalah source diagram; rendering di GitHub diperiksa saat tersedia, tidak diklaim sebagai uji aplikasi.
