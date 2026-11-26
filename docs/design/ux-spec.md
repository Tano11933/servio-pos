# Spesifikasi UX dan wireframe

Status: rancangan implementasi berdasarkan [DESIGN.md](../../DESIGN.md), [PRD](../product/prd.md), [aturan bisnis](../product/business-rules.md), dan [kontrak API](../api/README.md). Bukan UI berjalan. Bentuk layar di bawah konseptual; seluruh data ilustratif diberi label saat dipakai pada demo.

## UX-01: navigasi dan konteks

Kasir masuk ke Pesanan dengan konteks outlet, akun dan shift terlihat. Navigasi operasional: Pesanan, Meja, Riwayat, Shift. Owner/manager mendapat Katalog, Bahan & Resep, Stok, Laporan, dan Pengaturan sesuai izin. Dapur langsung ke Antrean dapur, bukan dashboard bisnis.

Navigasi hanya merutekan ke fitur yang telah diimplementasikan; spesifikasi tidak mengizinkan tautan mati. UI menghindari menu tak berizin, tetapi server tetap memeriksa akses. Tidak ada perpindahan outlet diam-diam saat ada draft atau hasil transaksi belum diketahui.

## UX-02: kasir utama

Wireframe tablet landscape:

```text
┌──────────────────────────────────────────────────────────────┐
│ Servio POS   Outlet / akun / shift           Status koneksi   │
├───────────┬────────────────────────────┬───────────────────────┤
│ Pesanan   │ Cari menu                  │ Takeaway / meja aktif │
│ Meja      │ Kategori yang tersedia     │ Draft / terkirim      │
│ Riwayat   │                            │ Item, opsi, catatan    │
│ Shift     │ Menu + harga + ketersediaan │ Jumlah dan rincian    │
│           │                            │ Total server          │
│           │                            │ Aksi sesuai tahap     │
└───────────┴────────────────────────────┴───────────────────────┘
```

Cari berlabel, fokus dapat dipanggil lewat kontrol yang terlihat. Kategori mempersempit hasil tanpa menghapus keranjang. Pilih menu membuka varian/modifier/catatan/jumlah; pilihan wajib dan min/max dipahami sebelum tambah. Menu nonaktif tidak dapat ditambahkan; harga server berbeda memerlukan konfirmasi ulang.

Daftar memisahkan “Belum dikirim” dari “Sudah dikirim”. Kontrol jumlah hanya untuk draft. Item terkirim menampilkan tahap dapur dan koreksi sesuai izin; tidak ada edit langsung yang mengubah snapshot. Ringkasan menyebut perubahan harga atau ketersediaan secara spesifik.

Urutan takeaway: buat draft → pilih item → tinjau → kirim batch → bayar penuh → struk. Dapur dapat mulai sebelum/sesudah pembayaran. Pembayaran dikunci jika ada item draft belum terkirim: pilih kirim atau buang draft dengan konfirmasi, bukan membayar item yang tidak masuk tagihan.

Pada ponsel, katalog dan keranjang terpisah. Tombol ringkasan menampilkan jumlah item dan total terakhir dari server; tampilkan “Perlu diperbarui” bila data kedaluwarsa. Kembali ke katalog mempertahankan pencarian dan draft, bukan membuat pesanan baru.

## UX-03: meja dan tagihan terbuka

```text
Outlet dan filter         Legenda berteks: Tersedia / Terpakai
[Nomor meja + status] [Nomor meja + status] [Nomor meja + status]
Pilih meja → tagihan aktif atau buat draft dine-in
```

Gunakan daftar/grid responsif bernomor, bukan denah drag-drop yang belum memiliki kebutuhan. Membuka meja terpakai memuat pesanan yang sama. Race pembukaan menampilkan pesanan yang sudah terbentuk setelah otorisasi, tidak memaksa tagihan kedua.

Batch tambahan tampil dengan waktu/identitas dan status dapur; mengirim tambahan tidak mengirim ulang batch lama. Setelah bayar, tagihan terkunci walaupun meja masih terpakai karena persiapan belum selesai. Teks “Sudah dibayar • Menunggu dapur” membedakan pembayaran dari meja tersedia. Aturan pelepasan meja masih mengikuti usulan BR-05.

## UX-04: dapur

```text
Antrean dapur   Outlet   Terakhir diperbarui   Status koneksi
Menunggu                     Diproses                   Siap
Nomor pesanan/meja            Nomor pesanan/meja         Ringkasan
Item, jumlah, modifier       Catatan                    Selesai disiapkan
[Mulai persiapan]            [Tandai siap]
```

Tablet lebar memakai kolom; lebar sempit memakai tab berlabel dan jumlah antrean, tanpa menyembunyikan pembaruan penting. Nomor, waktu urutan, mode/meja, catatan dan status pembatalan terlihat. Tanpa harga, saldo kas, atau akses pengguna. Tidak memakai warna saja untuk menandai mendesak/batal.

Polling aktif/reconnect mengambil snapshot; tampilkan waktu update terakhir. Saat koneksi gagal, daftar lama ditandai belum diperbarui dan perubahan status tidak disahkan lokal. Mulai/siap menunggu server, mencegah tap ganda, serta memulihkan konflik dengan status terkini. Kasir tidak mendapat tombol status dapur jika matriks izin tidak membolehkannya.

## UX-05: pembayaran dan struk

```text
Tagihan / meja                  Ringkasan item dan total server
Metode: Tunai | QRIS manual | EDC manual
Tunai: jumlah diterima → kembalian
Digital: konfirmasi pemeriksaan kasir; bukan status penyedia
[Kembali]                       [Terima pembayaran]
```

Tinjauan menampilkan gross, diskon, service, pajak, total dan mata uang sesuai konfigurasi. Kasir tidak memperoleh kontrol diskon bila hanya owner/manager yang berizin. Pilihan tarif tidak ditebak dari contoh dokumen.

Prasyarat: seluruh item yang akan dibayar telah dikirim, total positif, shift milik pelaku OPEN, versi total sudah dikonfirmasi. Jika tidak ada shift, arahkan “Buka shift” lalu kembali ke pesanan, tanpa membuat pembayaran baru.

Konfirmasi mengunci input dan mempertahankan identitas operasi sampai hasil diketahui. Status “Memeriksa hasil pembayaran” tidak memiliki tombol bayar baru. Setelah sukses tampil struk dari data tersimpan; “Cetak” dan “Cetak ulang” tidak membuat penjualan. Tombol cetak hanya tersedia bila jalur printer sudah terimplementasi; sebelum itu struk digital tetap nyata.

## UX-06: pembatalan dan retur

Pembatalan belum bayar: pilih item/baris atau seluruh pesanan, alasan wajib, ringkasan dampak dapur/reservasi/waste, konfirmasi berizin. Item draft milik pengguna dapat dibuang sesuai kebijakan; item terkirim tidak menggunakan izin draft. Jangan hanya menampilkan dialog “Yakin?” tanpa menjelaskan dampak.

Retur transaksi lunas: buka transaksi asal → item/kuantitas yang masih berhak → alasan/disposisi → nilai refund dari alokasi server → shift aktif pelaku → konfirmasi pengembalian manual → hasil tersimpan. Metode mengikuti transaksi asal, tidak bisa diubah menjadi nominal bebas.

Jika PREPARING, jelaskan prasyarat menunggu READY menurut usulan baseline. Pada refund digital, jelaskan “Catat pengembalian yang sudah dilakukan” dan risiko mencatat ulang setelah timeout; jangan mengklaim penyedia mengirim uang. Operator tanpa izin diarahkan meminta manager/owner beroperasi memakai akun sendiri, tanpa input password orang lain dalam modal kasir.

## UX-07: shift, riwayat, dan administrasi

Buka shift meminta saldo awal dengan mata uang jelas. Kas masuk/keluar meminta arah, nilai positif dan alasan. Tutup menampilkan expected, aktual, selisih, serta alasan bila tidak nol; tunai dan digital dibedakan. Setelah sukses CLOSED, tidak ada tombol mengubah saldo historis.

Riwayat menampilkan identitas transaksi, waktu outlet, mode/meja, metode, total, dan status refund. Detail menjadi sumber struk/koreksi, bukan salinan keranjang. Pencarian dan laporan menggunakan filter serta pagination server dan izin; jangan memuat data seluruh tenant di browser.

Administrasi katalog/resep/stok memakai tabel pada desktop dan daftar/detail pada ponsel; formulir satu kolom untuk input kompleks. Label satuan, dimensi, jumlah dan alasan wajib terlihat. Waste sesudah konsumsi diberi penjelasan “Tidak mengurangi stok lagi” bila hanya klasifikasi konsumsi. Laporan menunjukkan rentang waktu/zona waktu dan sumber angka, tanpa kartu statistik contoh yang terlihat nyata.

## UX-08: pemetaan kontrak dan gap

Perintah inti memakai REST order/batch/start/ready/cancellation/payment/refund/shift pada kontrak awal; UI tidak menentukan nama endpoint baru sendiri. [State pemulihan](interaction-states.md) menjabarkan error/conflict/retry.

Kontrak yang belum lengkap: list pesanan aktif, daftar/detail meja, GET detail shift, daftar riwayat, CRUD katalog/resep/stok, formulir membership/pengaturan, dan laporan/ekspor lengkap. Tambahkan request/response, permission, pagination dan error sebelum membuat kontrolnya. Jangan mengisi celah ini dengan endpoint rekaan atau data mock tanpa label.

Pemetaan: UX-01 → ACC-01/02/03; UX-02/03 → CAT-01/02 dan ORD-01/02/03/04; UX-04 → KIT-01; UX-05 → PAY-01/02/03/04, REC-01; UX-06 → CAN-01, RET-01, AUD-01; UX-07 → INV-01/02/03, SHF-01/02, REP-01/02; seluruh alur → UX-01 pada PRD dan RES-01/02. ID UX di heading dokumen adalah alur, bukan requirement baru yang mengganti ID PRD.
