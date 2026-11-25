# Review arsitektur

Status: review dokumen, bukan audit aplikasi atau pengujian database. Integrasi branch setelah pemeriksaan diotorisasi pemilik; status pilihan bisnis terbuka tidak diubah oleh merge.

## Cakupan

ARCH-01: diagram container dan batas modul. ARCH-02: ERD/kamus data/constraint. ARCH-03: ADR transaksi/idempotency/pemulihan. ARCH-04: REST prioritas dan strategi validasi.

## Temuan yang sudah diperbaiki dalam penyusunan

- **Row belum ada tidak bisa dilindungi hanya dengan row lock:** pembuatan saldo, meja/shift aktif, dan payment kini secara eksplisit memakai unique/insert-on-conflict serta lock/revalidasi yang sesuai.
- **Refund dan dapur bisa memakai versi berbeda:** REST start/ready/refund sekarang menjelaskan expectedVersion order, serta penggunaan kuantitas aktif setelah refund parsial.
- **Draft tidak memerlukan izin koreksi manager:** kontrak cancellation membedakan pembuat/peran draft dari owner/manager untuk item terkirim.
- **CHECK lintas row tidak menjamin total:** model membedakan CHECK satu row dari validasi alokasi/refund/waste di bawah transaksi/lock, serta unique hak refund unit.
- **Hasil COMMIT hilang berbeda dari rollback:** ADR memisahkan bounded retry setelah rollback pasti dari lookup operasi saat hasil commit ambigu. Lookup tidak ditemukan bukan bukti request lain telah gagal.
- **Scope dan pencabutan akses harus berlaku juga untuk replay:** query/FK komposit, pemeriksaan izin saat lookup/replay, dan shared/exclusive access lock tertulis.
- **Katalog belum terpetakan pada strategi pengujian awal:** pemetaan CAT-01/02 ditambahkan untuk menu nonaktif, varian/modifier, manipulasi harga, dan quote kedaluwarsa.

## Pemeriksaan penyerahan

Validasi mencakup tautan lokal, whitespace, JSON contoh, fence diagram Mermaid dan relasi entitas, referensi requirement, pola label/atribusi, serta metadata commit. Source diagram tersedia untuk rendering GitHub; tidak ada klaim screenshot/render visual telah diperiksa.

Hasil pemeriksaan lokal: 25 dokumen Markdown, 99 tautan lokal valid, satu blok JSON valid, dua blok source diagram dengan fence berpasangan, 31 requirement terpetakan pada strategi pengujian, dan 21 baris operasi API unik. Pemeriksaan source diagram bukan rendering Mermaid. Metadata commit dan kecocokan remote diperiksa terpisah sesudah commit/push.

## Batas yang masih berlaku

- Belum ada schema Prisma, migrasi, source aplikasi, endpoint berjalan, test runner atau deployment.
- FK/partial unique, locking, replay, revoke, restore, printer dan beban masih perlu bukti runtime menurut strategi pengujian.
- O-001–05 tetap usulan; presisi kuantitas, konfigurasi tarif/outlet, lisensi, hosting, dan versi dependency belum dikunci.
- REST saat ini mencakup operasi prioritas; JSON Schema/OpenAPI lengkap dan endpoint katalog/inventori/ekspor harus diselesaikan sebelum fiturnya dibuat.
- Review dokumen tidak membuktikan jaminan produksi. Tidak ada temuan dokumentasi yang dipertahankan sebagai penghambat integrasi; dependensi implementasi di atas tetap terbuka.
