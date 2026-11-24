# Aturan uang, shift, dan retur

Status: usulan baseline, menunggu review. Melengkapi [aturan operasional](business-rules.md) dan PRD PAY-01/02/03/04, SHF-01/02, RET-01, REC-01, REP-01/02, serta AUD-01. Persentase contoh adalah data ilustrasi, bukan ketentuan pajak atau nilai yang sudah disetujui outlet.

## BR-07: nilai uang dan urutan hitung

Usulan pilot memakai IDR, bilangan bulat rupiah, harga belum termasuk pajak/service charge, dan pembulatan half-up ke satu rupiah. Nilai uang di API tidak boleh kehilangan presisi; kontrak representasi diputuskan pada arsitektur. Perhitungan rasional/desimal dilakukan eksak sebelum pembulatan. Tidak menggunakan float biner biasa.

Kuantity menu bilangan bulat positif; kuantitas bahan mengikuti presisi satuan. Harga unit termasuk varian dan modifier yang dipilih. Diskon pilot hanya satu diskon tingkat tagihan, berupa nominal rupiah atau persentase, tanpa stacking. Konfigurasi diskon dan izin pemberiannya harus dikunci sebelum implementasi; usulan awal hanya owner/manager dapat menetapkan diskon. Kasir memakai harga/konfigurasi yang tersimpan.

Urutan untuk item aktif:

1. `gross = Σ(quantity × harga_unit_snapshot)`.
2. Diskon nominal harus 0..gross; diskon persentase 0..100, hasil nominal dibulatkan half-up. Diskon melebihi gross ditolak, bukan diam-diam dikoreksi.
3. `net = gross − discount`.
4. `service = round_half_up(net × service_rate)`.
5. Usulan `tax = round_half_up((net + service) × tax_rate)`.
6. `total = net + service + tax`. Tidak ada pembulatan kas tambahan berdasarkan kelipatan uang tunai; usulan ini berlaku sama untuk tunai/digital.

Owner mengatur rate nonnegatif maksimal 100%; default data contoh 0%. Pengaturan basis pajak di atas adalah usulan produk yang wajib diperiksa terhadap kebutuhan outlet sebelum dikunci. Jangan menganggapnya keputusan kewajiban perpajakan.

Pembayaran mengunci seluruh snapshot dan total dalam transaksi yang sama. Sebelum bayar, konfirmasi ulang bila item/versi/total telah berubah; sesudah PAID tidak ada perubahan tagihan. Setelah refund, harga historis tidak dihitung ulang.

### Alokasi untuk retur parsial

Alokasikan discount proporsional gross tiap baris, service proporsional net baris, dan tax proporsional net+service baris. Gunakan hasil lantai tiap bagian lalu distribusikan sisa rupiah berdasarkan pecahan terbesar, tie-break ID baris stabil. Jumlah alokasi harus sama tepat dengan total komponen tagihan.

Alokasikan komponen setiap baris ke unit menu dengan aturan sisa yang sama, tie-break urutan unit. Simpan hak refund per unit. Retur kuantitas memakai unit belum diretur secara berurutan, sehingga nilai kumulatif tidak melebihi hak semula dan refund terakhir menghabiskan sisa tepat. Modifier melekat pada harga unit; tidak direfund terpisah. Refund unit mencakup net, service, dan tax yang dialokasikan; tidak ada input nilai refund bebas.

## BR-08: pembayaran dan shift

Status pembayaran: `UNPAID` → `PAID` → `PARTIALLY_REFUNDED` → `REFUNDED`. Perintah ditolak tidak memindahkan status; status tidak diketahui di klien bukan status gagal di server. Partial refund diperbolehkan sebagai koreksi setelah pelunasan, bukan pembayaran sebagian.

- Satu tagihan hanya boleh memiliki satu pembayaran sukses penuh. Kunci pesanan dan verifikasi versi/total serta shift dalam transaksi pembayaran.
- Tunai: jumlah diterima wajib >= total; `change = received − total`. Yang masuk kas penjualan adalah total, bukan received.
- QRIS/EDC: nominal harus total; kasir mencatat konfirmasi manual. Tidak ada integrasi penyedia atau jaminan penyelesaian otomatis.
- Per pengguna/outlet paling banyak satu shift OPEN. Shift tidak dibagi/dipindahkan antarpengguna pada pilot. Owner/manager yang menerima pembayaran juga harus membuka shift sendiri.
- Membuat/mengirim pesanan memerlukan izin, tetapi tidak mengharuskan shift; menerima pembayaran dan melakukan mutasi kas memerlukan shift OPEN milik pelaku pada outlet sama.
- Kas masuk/keluar nonpenjualan memerlukan nominal positif dan alasan; bukan pengganti pencatatan pembayaran/refund.
- Tutup shift mencatat hitungan aktual nonnegatif, expected, dan selisih. Selisih wajib diberi alasan jika tidak nol. Tidak membuka kembali/mengubah shift CLOSED.
- Penutupan mengunci shift sehingga pembayaran, refund, dan mutasi bersamaan harus masuk sebelum penutupan atau ditolak sesudahnya. Tidak ada mutasi terlambat ke shift tertutup.

`expected_cash = opening_cash + cash_sales + cash_in − cash_refunds − cash_out`.

Expected cash tidak boleh negatif pada baseline ini; pengeluaran/refund tunai yang membuatnya negatif ditolak dan memerlukan kas masuk yang sah. QRIS/EDC tidak menambah/mengurangi kas fisik. Catatan penutupan membedakan seluruh metode pembayaran.

## BR-09: retur dan refund manual

Owner/manager berizin memproses retur dengan alasan, transaksi/item asal, kuantitas, disposisi, dan metode asal. Refund mengikuti metode pembayaran asal; penggantian metode di luar baseline. Manager/owner pelaku membuka shift sendiri pada outlet asal, termasuk untuk pencatatan refund digital. Kasir tanpa izin tidak dapat menyetujuinya menggunakan akun orang lain.

Hak refund berasal dari alokasi BR-07. Validasi batas kumulatif jumlah/nilai dilakukan dengan locking dan idempotency. Setelah semua unit diretur, jumlah refund = total yang dibayar. Audit dan ledger kas/refund/penyesuaian dibuat atomik dengan status refund.

- Refund tunai mengurangi kas shift pelaku yang masih OPEN, bukan mengubah shift penjualan lama.
- Refund QRIS/EDC mencatat konfirmasi pengembalian manual di luar sistem; tidak memberi label bahwa penyedia sudah mengirim uang. Operator harus menyelesaikan pengembalian eksternal sebelum mengonfirmasi pencatatannya. Catat referensi aman bila tersedia.
- Retry dengan identitas sama hanya mengembalikan catatan yang sama; jangan mengulang pengembalian uang eksternal berdasarkan timeout aplikasi. Jika pengembalian eksternal selesai tetapi pencatatan gagal, periksa hasil/referensi terlebih dahulu dan eskalasi rekonsiliasi bila hasil tidak dapat dipastikan.
- Unit belum disiapkan dilepas reservasinya dan dibatalkan sesuai BR-06; unit PREPARING ditunda sampai READY. Unit READY tidak otomatis mengembalikan bahan; jika dibuang, waste merujuk konsumsi asal tanpa pengurangan kedua.
- Disposisi pilot: `NOT_PREPARED`, `DISCARDED`, atau `NO_STOCK_CHANGE`. Disposisi tidak menciptakan restock bahan otomatis.

Usulan pilot tidak membatasi retur berdasarkan jumlah hari, tetapi mengharuskan transaksi sumber masih tersimpan dan semua validasi di atas terpenuhi. Kebijakan usaha lebih ketat dapat diputuskan pemilik sebelum implementasi; jangan menghapus transaksi sumber untuk mengakali batas.

## BR-10: laporan dan audit

Penjualan dicatat pada waktu pelunasan; refund pada waktu refund, tanpa menulis ulang laporan shift/periode lama. Tampilkan gross, diskon, service, tax, penjualan lunas, refund, serta net setelah refund secara terpisah; nilai tunai diterima dan kembalian bukan omzet.

Tanggal bisnis menggunakan zona waktu outlet dan rentang awal inklusif/akhir eksklusif. Simpan timestamp yang tidak ambigu; perubahan zona waktu tidak mengubah snapshot transaksi lama. Rekonsiliasi antarperiode membedakan refund dari penjualan asal.

Audit mencatat pelaku, tenant/outlet, waktu, jenis operasi, sumber, alasan, dan nilai sebelum/sesudah yang diperlukan. Jangan mencatat kredensial atau session token. Pembatalan/retur tidak menghapus transaksi asal; akses audit mengikuti matriks izin.

## Keputusan review

Konfirmasi IDR/pembulatan, basis service/pajak, diskon owner/manager saja, alokasi refund termasuk biaya, satu shift per pengguna/outlet, refund ke shift pelaku, dan kebijakan masa retur. Nilai tarif serta data outlet belum ditentukan. Semua usulan ini tetap terbuka di daftar keputusan sampai pemilik menyetujuinya.
