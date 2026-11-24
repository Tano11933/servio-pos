# Matriks validasi kebutuhan

Status: rancangan pengujian untuk [PRD](prd.md), bukan hasil pengujian. Semua skenario di bawah **belum dijalankan** karena aplikasi belum tersedia.

| Skenario | Requirement | Bukti yang diperlukan |
| --- | --- | --- |
| VAL-01: login salah, logout, sesi kedaluwarsa, membership dicabut | ACC-01, ACC-03 | Integration/API: request ditolak setelah sesi/akses berakhir |
| VAL-02: ganti ID tenant/outlet pada pesanan, bahan, relasi, file, struk, ekspor | ACC-02, REP-02 | Integration/API: akses ditolak dan tidak ada perubahan; jalur background ikut diperiksa |
| VAL-03: role tanpa izin memanggil endpoint sensitif langsung | ACC-02, ACC-03, AUD-01 | API: penolakan default sesuai matriks akses; audit perubahan izin |
| VAL-04: menu nonaktif, modifier tidak sah, harga klien dimanipulasi | CAT-01, CAT-02, PAY-01 | Unit dan API: validasi server dan hasil hitung benar |
| VAL-05: konversi g/kg, ml/l, nilai nol/negatif dan lintas dimensi | INV-01 | Unit: hasil eksak; kasus tidak sah ditolak |
| VAL-06: ubah resep/harga setelah pesanan dikunci | INV-02, PAY-01, ORD-01 | Integration: snapshot historis tetap; transaksi baru memakai versi yang berlaku |
| VAL-07: stok awal, penerimaan, penyesuaian, waste, rekonsiliasi ledger | INV-03, REP-02 | PostgreSQL integration: saldo dan sumber pergerakan cocok; kegagalan rollback |
| VAL-08: takeaway lengkap dengan modifier dan catatan | ORD-01, KIT-01, PAY-02, REC-01 | E2E: item dapur, pembayaran, struk, dan sumber stok terlacak |
| VAL-09: dine-in dan batch tambahan, dua perangkat mengubah versi sama | ORD-02, ORD-03, KIT-01 | E2E/API: tambahan hanya sekali; konflik tertangani tanpa item hilang |
| VAL-10: dua pesanan memperebutkan bahan terakhir | ORD-04, INV-03 | PostgreSQL concurrency: perubahan atomik, batas stok terpenuhi, ledger tidak ganda |
| VAL-11: putus/reconnect layar dapur dan request status berulang | KIT-01, ORD-04 | Integration/E2E: antrean terbaru, transisi sah, efek bahan tidak berulang |
| VAL-12: diskon/pajak/service charge dan pembulatan dengan contoh yang disepakati | PAY-01 | Unit: hasil eksak untuk batas nilai dan urutan hitung; konfigurasi invalid ditolak |
| VAL-13: tunai kurang/pas/lebih, QRIS/EDC konfirmasi manual | PAY-02, SHF-02 | Unit/E2E: kembalian benar; digital tidak menambah kas fisik |
| VAL-14: klik ganda, key sama payload sama/berbeda, dua key membayar satu tagihan | PAY-03 | PostgreSQL concurrency/API: hanya satu pelunasan; replay stabil, konflik ditolak |
| VAL-15: respons hilang sesudah commit dan retry setelah restart | PAY-04, RES-01, RES-02 | Fault injection: lookup memulihkan hasil tanpa transaksi baru |
| VAL-16: bayar bersamaan dengan tutup shift; mutasi pada shift tertutup | SHF-01, SHF-02 | PostgreSQL concurrency: operasi konsisten; saldo dapat direkonsiliasi |
| VAL-17: batal sebelum/sesudah persiapan, ulang request, tanpa izin/alasan | CAN-01, ORD-04, AUD-01 | API/integration: aturan stok/dapur sah; audit ada; operasi ditolak tanpa efek |
| VAL-18: retur parsial/berlebih/bersamaan dan refund manual | RET-01, SHF-02, AUD-01 | PostgreSQL integration: batas kumulatif terjaga, kas terlacak, resep tidak dibalik otomatis |
| VAL-19: laporan dibanding transaksi, retur, waste, dan ledger; batas tanggal outlet | REP-01, REP-02 | Integration: jumlah tepat, filter zona waktu benar, akses ekspor dibatasi |
| VAL-20: keyboard, sentuhan, loading, kosong, validasi, error, koneksi putus | UX-01 | E2E dan pemeriksaan perangkat: fokus terlihat dan alur dapat diselesaikan |
| VAL-21: refresh, logout/ganti akun/outlet, pembaruan PWA saat ada draft | RES-01, RES-02 | E2E/Android: draft tidak bocor, tidak hilang tanpa penanganan, tidak mengesahkan offline |
| VAL-22: printer mati/terputus, cetak ulang, lebar kertas terpilih | PRT-01, REC-01 | Uji fisik: model/koneksi dicatat, struk terbaca, tidak menambah penjualan |
| VAL-23: deploy/migrasi gagal, restart layanan, log/error/health | OPS-01, PAY-04 | Staging/runbook: langkah pemulihan terbukti, log tanpa secret |
| VAL-24: restore backup terpisah dan verifikasi data penting | OPS-02 | Bukti restore: waktu aktual, titik data pulih, jumlah/relasi/sampel transaksi diverifikasi |
| VAL-25: beberapa kasir dan data target outlet | OPS-03 | Pengukuran beban sesuai target yang disepakati; hasil dan temuan disimpan |

## Ketentuan bukti

[Contoh penerimaan aturan](business-examples.md) memperinci expected result untuk VAL-02/03/07/09/10/11/12/13/14/15/16/17/18/19 berdasarkan usulan BR-01 sampai BR-10. Pilihan bisnis yang belum disetujui tidak diperlakukan sebagai aturan final.

- Unit test tidak cukup untuk membuktikan locking, constraint, rollback, atau isolasi PostgreSQL. Gunakan database nyata yang terisolasi untuk pengujian tersebut.
- Setiap pelaksanaan mencatat versi/commit, lingkungan, data uji aman, hasil aktual, serta temuan. Jangan memasukkan secret atau data pelanggan ke repository.
- Nilai uang/pajak dan transisi stok yang belum disepakati tidak ditebak dalam expected result; kunci aturan lebih dahulu.
- Setelah fitur berubah, jalankan pengujian yang relevan dan regresi kasus kritis terdampak.
- Penerimaan akhir memerlukan latihan outlet, printer, dan restore fisik/operasional yang tidak dapat digantikan dengan mock.
