# State interaksi dan pemulihan

Status: spesifikasi; belum diuji pada aplikasi. Mengikuti [DESIGN.md](../../DESIGN.md), [UX](ux-spec.md) serta kontrak API. Teks pesan di bawah adalah usulan copy operasional.

| State | Tampilan dan aksi | Perilaku wajib |
| --- | --- | --- |
| Loading pertama | “Memuat menu/pesanan…” dan indikator berlabel | Jangan menampilkan saldo/total nol sebagai hasil nyata |
| Refresh latar | Data terakhir + penanda pembaruan | Jangan menghapus keranjang atau memindah fokus |
| Hasil kosong | “Belum ada pesanan” atau “Menu tidak ditemukan” | Bedakan katalog kosong dari filter tanpa hasil; tawarkan reset filter yang berfungsi |
| Validasi input | Pesan di dekat field dan ringkasan jika perlu | Hubungkan label/error secara programatik; fokus ke field bermasalah, input tetap tersimpan |
| 401 sesi berakhir | “Sesi berakhir. Masuk kembali.” | Bersihkan data sensitif/cache; jangan replay mutasi otomatis setelah login; lookup hasil lama memakai identitas/izin sah |
| 403 akses berubah | “Akses untuk tindakan ini tidak tersedia.” | Nonaktifkan tindakan, muat izin terbaru; jangan menawarkan akun orang lain |
| 404 sumber hilang | “Pesanan tidak tersedia di outlet ini.” | Kembali ke daftar berizin tanpa mengungkap tenant lain |
| Konflik versi | “Pesanan sudah diperbarui. Tinjau perubahan terbaru.” | Muat snapshot, tampilkan perubahan jumlah/total; jangan overwrite atau resend otomatis |
| Stok tidak cukup | “Bahan tidak cukup untuk mengirim pesanan.” | Batch gagal utuh; pertahankan draft untuk diperbaiki, tampilkan rincian aman dari server |
| Quote berubah | Harga/resep/ketersediaan baru dan pilihan konfirmasi | Jangan mengganti harga terkirim atau mengirim tanpa persetujuan |
| Request dikirim | “Menyimpan…”; aksi terkait diblokir sementara | Simpan key dan payload operasi; menonaktifkan tombol bukan satu-satunya perlindungan |
| Timeout/hasil ambigu | “Hasil belum diketahui. Sedang diperiksa.” | Lookup hasil dengan key sama; tidak ada “Bayar lagi” atau key baru |
| Lookup belum ditemukan | “Hasil belum dapat dipastikan.” + “Periksa lagi” | 404 bukan bukti rollback; retry perintah hanya key/payload sama sesuai protokol, bukan transaksi baru |
| Gagal yang pasti rollback | Pesan prasyarat/field yang relevan | Perbaikan payload menjadi tindakan baru hanya setelah hasil lama pasti; hindari loop retry bisnis |
| 429 atau retryable | “Coba kembali setelah beberapa saat.” | Hormati backoff/Retry-After dan batas retry, tetap simpan operasi lama jika hasil belum pasti |
| Offline | Banner “Koneksi terputus”; waktu update terakhir | Draft lokal boleh diedit; pengiriman, bayar, status dapur dan mutasi stok/shift tidak disahkan offline |
| Reconnect | “Memperbarui data…” | Revalidasi izin/status/quote dan selesaikan hasil tertunda sebelum operasi baru |
| Cetak gagal | “Transaksi tersimpan. Struk belum tercetak.” + cetak ulang | Tidak refund/ulang bayar otomatis; cek printer atau buka struk digital |
| Pembaruan PWA | “Pembaruan siap” dengan pilihan aman | Tunda reload saat command berlangsung atau hasil belum diketahui; draft tidak hilang diam-diam |

## Draft dan identitas operasi

Draft lokal dipisah pengguna/tenant/outlet. Draft setelah refresh bukan transaksi sah; revalidasi harga, bahan dan order/version server sebelum kirim. Jika penyimpanan browser gagal, beri tahu bahwa draft belum tersimpan lokal dan jangan menjanjikan pemulihan.

Logout menghapus draft/cache sensitif sesuai baseline arsitektur. Jika ada draft, minta konfirmasi bahwa draft lokal akan dihapus; opsi kembali tidak logout. Jika sesi berakhir tanpa tindakan pengguna, data lokal sensitif tetap dibersihkan. Jangan menjanjikan draft dapat dipulihkan sesudah logout/ganti akun.

Identitas operasi tertunda bukan token autentikasi. Payload/outcome yang sensitif tidak dicache oleh service worker. Catat operation key/resource secara terisolasi secukupnya untuk lookup; saat login ulang aktor sama, server tetap memeriksa izin terkini. Jika akses sudah dicabut, eskalasi ke owner yang berizin melalui riwayat transaksi, bukan membuka hasil ke pengguna lama.

Pindah outlet dan keluar aplikasi menampilkan peringatan untuk operasi belum pasti. Jangan menganggap menutup dialog/tab membatalkan perintah di server. Protokol pemulihan mengikuti ADR transaksi, bukan tombol browser Back.

## Fokus, pengumuman, dan perangkat

Perubahan antrean dapur tidak merebut fokus tombol yang sedang digunakan. Status pembayaran/pengiriman menggunakan area live polite dengan pesan ringkas. Error yang menghalangi langkah berikutnya terlihat tanpa hanya toast sementara. Dialog modal punya nama, fokus awal, trap, Escape aman, dan return focus.

Uji viewport 360, 768, 1024, 1280px; landscape tinggi pendek; keyboard virtual terbuka; zoom 200%; long text; menu tanpa foto; item/modifier banyak. Sticky footer menyisihkan ruang dan safe-area. Tidak ada kontrol hanya hover, gesture, warna atau ikon tanpa nama. Horizontal overflow halaman tidak boleh dipakai untuk menyelesaikan kolom yang tidak muat; tabel rinci boleh punya scroll region berlabel bila reflow tidak cukup.

## Rencana bukti runtime

Tambahkan kasus E2E sesuai VAL-08/09/11/13/14/15/16/17/18/20/21/22 sebelum fitur dinyatakan siap. Uji pembaca layar/keyboard, Android sentuh, print fisik dan koneksi terganggu saat implementasi tersedia. Pemeriksaan dokumen dan rasio warna saja tidak membuktikan seluruh state sudah bekerja.
