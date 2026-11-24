# Contoh penerimaan aturan bisnis

Status: expected result untuk review, bukan pengujian aplikasi yang telah dijalankan. Mengacu [BR-01 sampai BR-06](business-rules.md) dan [BR-07 sampai BR-10](settlement-rules.md). Nilai tarif ilustratif.

| ID | Aturan | Kondisi dan tindakan | Expected result |
| --- | --- | --- | --- |
| EX-01 | BR-01, BR-02 | Dua perangkat mengirim perubahan dengan versi sama | Satu berhasil; lainnya konflik, tanpa item hilang atau duplikasi batch |
| EX-02 | BR-04 | Susu 1.000 ml; kirim 200 ml | on_hand 1.000, reserved 200, tersedia 800 |
| EX-03 | BR-03, BR-04 | EX-02 mulai persiapan lalu batal sebelum bayar | on_hand 800, reserved 0; konsumsi 200 terkait waste sekali |
| EX-04 | BR-04 | EX-02 batal saat QUEUED | on_hand 1.000, reserved 0; tanpa konsumsi/waste |
| EX-05 | BR-01, BR-04 | Bahan tersedia 200 ml; dua batch masing-masing memerlukan 200 ml | Hanya satu reservasi; batch kalah ditolak utuh |
| EX-06 | BR-02, BR-05 | Pesanan PAID tetapi item masih QUEUED | Tagihan terkunci, meja masih terpakai; item READY terakhir menutup pesanan dan melepas meja |
| EX-07 | BR-02, BR-03 | Replay pengiriman/status yang sebelumnya berhasil | Hasil operasi sama; tidak membuat tiket atau konsumsi baru |
| EX-08 | BR-07 | Gross 40.000, diskon 10%, service 5%, tax 10% | Diskon 4.000; net 36.000; service 1.800; tax 3.780; total 41.580 |
| EX-09 | BR-07 | Baris gross 1.000 dan 2.000, diskon nominal 100, tanpa biaya lain | Alokasi diskon 33 dan 67; net 967 dan 1.933; total 2.900 |
| EX-10 | BR-07, BR-09 | Satu baris 3 unit gross masing-masing 1.000, diskon 100, tanpa biaya | Hak refund berurutan 966, 967, 967; refund semua tepat 2.900 |
| EX-11 | BR-07 | Net 101, service 0%, tax 0,5% | Tax 1 (half-up dari 0,505); total 102 |
| EX-12 | BR-08 | Total 41.580; tunai diterima 50.000 | Kembalian 8.420; kas penjualan bertambah 41.580 |
| EX-13 | BR-08 | Total 41.580; tunai diterima 40.000 | Ditolak tanpa pembayaran atau mutasi kas |
| EX-14 | BR-01, BR-08 | Dua request berbeda membayar tagihan sama, atau retry setelah respons hilang | Satu pelunasan; replay key sama payload sama stabil; key sama payload berbeda ditolak |
| EX-15 | BR-08 | Saldo awal 100.000, penjualan tunai 41.580, kas masuk 20.000, refund tunai 10.000, kas keluar 5.000 | Expected 146.580; aktual 146.000 menghasilkan selisih −580 dan wajib alasan |
| EX-16 | BR-08, BR-09 | Refund penjualan lama sesudah shift asal ditutup | Shift asal tidak berubah; refund tunai mengurangi shift aktif pelaku di outlet sama |
| EX-17 | BR-09 | Dua refund memperebutkan unit terakhir yang boleh diretur | Satu berhasil; lainnya ditolak/replay sesuai identitas; akumulasi tidak melampaui hak |
| EX-18 | BR-06, BR-09 | Refund 1 dari 2 unit QUEUED, kemudian dapur mulai | Hanya 1 unit tersisa disiapkan; reservasi unit direfund dilepas sekali |
| EX-19 | BR-06, BR-09 | Refund item PREPARING atau kas refund melebihi expected | Ditolak tanpa mutasi; operator menyelesaikan prasyarat yang sah |
| EX-20 | BR-01, BR-10 | Pengguna outlet/tenant lain mencoba sumber transaksi/refund | Ditolak tanpa efek, termasuk replay dan ekspor |
| EX-21 | BR-08 | Penutupan shift bersamaan dengan pembayaran | Pembayaran masuk sebelum tutup atau ditolak setelah tutup; expected tidak tertinggal |
| EX-22 | BR-10 | Penjualan periode sebelumnya direfund pada periode sekarang | Periode lama tetap; periode sekarang menampilkan refund terhubung sumber |

Pemetaan ke matriks validasi: EX-01/06/07 → VAL-09/11; EX-02–05 → VAL-07/10/17; EX-08–13 → VAL-12/13; EX-14 → VAL-14/15; EX-15/16/21 → VAL-16/18; EX-17–19 → VAL-18; EX-20 → VAL-02/03; EX-22 → VAL-19. Semua validasi locking/rollback memerlukan PostgreSQL nyata saat implementasi tersedia.
