# Arah desain Servio POS

Status: arah visual dari pemilik telah dikonfirmasi; spesifikasi turunan di bawah menjadi baseline dokumentasi untuk implementasi dan review. Belum ada UI berjalan atau pengujian perangkat.

## Arah yang disetujui

- Terang dan hangat, bernuansa kafe dengan aksen hijau.
- Tablet Android landscape dengan sentuhan menjadi perangkat kasir utama; desktop dan ponsel tetap mendapat layout responsif.
- Pemeriksaan kualitas dilakukan selama penyusunan, bukan ditunda sampai akhir.

Karakter: ramah, tenang, terbaca dalam operasi sibuk. Fokus setiap layar adalah pekerjaan berikutnya, bukan dekorasi. Tidak ada logo/ilustrasi/foto yang dibuat tanpa brief tambahan; gunakan nama teks Servio POS dan nama menu nyata yang disediakan outlet. Data demonstrasi wajib diberi label data contoh.

## Prinsip visual

ENERGY 1 / RHYTHM 2 / MOTION 1. Antarmuka operasional yang tenang dengan variasi sesuai fungsi: katalog berbentuk grid, keranjang daftar, meja ringkasan posisi, dapur antrean, laporan tabel. Animasi hanya umpan balik tombol/perubahan panel; hormati reduced motion dan jangan menunda input.

Identitas berulang: latar krem hangat, permukaan putih, aksen hijau pekat pada tindakan utama, judul pendek, dan angka sejajar. Layout mengikuti aktivitas, bukan pola landing page. Mode terang merupakan pilihan pemilik dan cocok untuk area kasir terang; dark mode di luar baseline, bukan toggle yang belum berfungsi.

## Token turunan

Nilai berikut dipilih dari arah pemilik sebagai baseline; bukan pilihan kode warna yang disebutkan langsung oleh pemilik.

| Token | Nilai | Tujuan |
| --- | --- | --- |
| canvas | `#F7F5EF` | Kehangatan kafe tanpa tekstur yang mengganggu |
| surface | `#FFFFFF` | Memisahkan area kerja dan meningkatkan keterbacaan |
| text | `#242C28` | Teks utama pada canvas/surface |
| text-muted | `#58645C` | Metadata yang tetap terbaca |
| primary | `#1F5A42` | Aksi utama dan fokus aktif |
| primary-hover | `#174532` | Umpan balik penunjuk; sentuhan tetap punya pressed state |
| focus | `#1F5A42` | Outline fokus dengan offset agar kontras terlihat |
| control-border | `#748078` | Batas input/kontrol yang perlu dikenali |
| divider | `#DEDCD5` | Pemisah dekoratif; tidak dipakai sebagai satu-satunya batas kontrol |
| danger | `#A1252E` | Kesalahan dan tindakan destruktif berlabel |
| warning | `#775219` | Perhatian berlabel, misalnya koneksi terganggu |

Warna semantik danger/warning adalah pengecualian fungsional; tidak menjadi aksen dekorasi. Tombol primary memakai teks putih; secondary memakai teks primary di permukaan putih. Status selalu memiliki teks, bukan warna saja. Jangan memakai teks putih pada latar warning pucat tanpa pemeriksaan.

Tipografi baseline memakai `system-ui, sans-serif` agar mengikuti font yang terpasang di perangkat kasir, mengurangi ketergantungan jaringan, dan mendukung glyph Bahasa Indonesia. Uang memakai tabular numerals, bukan font monospace dekoratif. Body/input minimal 1rem; label tetap terbaca, metadata minimal 0.875rem; judul layar sekitar 1.25–1.5rem. Ikuti pengaturan font pengguna dan uji zoom 200%.

Skala jarak 4/8/12/16/24/32px. Jarak antarkontrol sentuh minimal 8px, target utama 48×48px atau lebih. Sudut input/tombol 8px, panel 12px; badge status bukan kapsul seragam. Elevasi hanya drawer/dialog yang menutupi konteks; panel biasa cukup pemisahan permukaan. Tidak ada gradient, glow, grid dekoratif, glassmorphism atau animasi masuk massal.

Ikon hanya bila menunjang aksi nyata (cari, tambah, kurangi, cetak, tutup) dengan label/accessible name. Jangan memilih paket ikon sebelum kebutuhan dan lisensi diperiksa saat scaffold. Penamaan tombol: “Kirim ke dapur”, “Terima pembayaran”, “Mulai persiapan”, “Tandai siap”, “Tutup shift”, “Cetak ulang”. Hindari istilah teknis request/version/idempotency pada alur pengguna.

## Layout dan hierarki

- Kasir: pesanan aktif dan total selalu jelas; satu aksi utama sesuai tahap. “Kirim ke dapur” untuk item draft, “Terima pembayaran” hanya jika seluruh item telah terkirim dan prasyarat terpenuhi.
- Dapur: urutan waktu dan item/catatan jadi fokus, tanpa nominal pembayaran atau laporan usaha.
- Laporan: angka dan sumber transaksi jadi fokus; tidak ada statistik/testimoni klaim produk yang dibuat-buat.
- Tindakan destruktif terpisah dari aksi rutin; alasan serta ringkasan dampak muncul sebelum konfirmasi.

| Lebar konten | Kasir | Navigasi dan panel |
| --- | --- | --- |
| >= 1024px | Katalog dan keranjang dua kolom; keranjang sekitar 360–400px jika ruang cukup | Sidebar ringkas; katalog min-width 0, grid menyesuaikan ruang |
| 768–1023px | Dua panel bila katalog >=360px dan keranjang >=320px; bila tidak muat gunakan mode satu panel | Menu navigasi berlabel; keranjang dapat dibuka tanpa kehilangan pencarian |
| < 768px | Satu panel: katalog, detail item, keranjang, pembayaran sebagai langkah terpisah | Ringkasan keranjang sticky dengan safe-area dan ruang konten yang dicadangkan |

Breakpoint menyesuaikan isi, bukan asumsi tipe perangkat. Pada tinggi pendek/keyboard terbuka, dialog/panel harus scroll dan aksi akhir tetap dapat dicapai. Jangan menutup overflow untuk menyembunyikan masalah layout.

## Aksesibilitas dan states

Target teks normal minimal kontras 4,5:1; teks besar dan batas/fokus kontrol minimal 3:1 terhadap latar yang bersebelahan. Pasangan token diuji secara numerik pada dokumen; hasil UI aktual tetap harus diuji saat implementasi.

Tab mengikuti urutan visual; Enter/Space mengaktifkan kontrol, Escape menutup dialog aman. Dialog menahan fokus dan mengembalikannya ke pemicu; saat pembayaran dikirim, Escape tidak membatalkan transaksi di server. Jangan mengandalkan hover, drag, warna, atau bunyi sebagai satu-satunya cara beroperasi. Announcement perubahan status tidak mengulang seluruh layar.

Sediakan loading, kosong, validasi, gagal, akses dicabut, konflik, dan hasil pembayaran belum diketahui. Empty state menjelaskan tindakan yang relevan; tanpa tombol mati. Data dashboard yang belum didukung endpoint tidak ditampilkan sebagai data nyata.

## Spesifikasi terkait dan batas review

[UX dan wireframe](docs/design/ux-spec.md), [state pemulihan](docs/design/interaction-states.md), dan [review desain](docs/design/review.md) menjadi acuan implementasi. Semua wireframe adalah struktur konseptual, bukan screenshot aplikasi atau bukti usability sudah lolos. Keputusan bisnis/izin yang masih usulan tetap mengikuti daftar keputusan; desain tidak menguncinya otomatis.
