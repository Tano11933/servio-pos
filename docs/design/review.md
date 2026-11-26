# Review desain dan UX

Status: review spesifikasi, bukan hasil pengujian UI. Pemilik mengotorisasi pemeriksaan, commit, push dan merge setelah validasi. Arah yang disepakati: terang/hangat bernuansa kafe, aksen hijau, tablet Android landscape; pemeriksaan dilakukan selama penyusunan.

## Hasil review

- Arah pemilik dipisahkan dari nilai token turunan pada DESIGN.md. Tidak ada logo/foto/testimoni atau statistik fiktif yang dibuat.
- Navigasi dan aksi menyesuaikan izin; dokumen tidak memberi kasir hak status dapur, pembatalan terkirim atau refund tanpa izin.
- Wireframe memisahkan item draft dan terkirim; pembayaran ditahan sampai draft dikirim atau dibuang agar item tidak terlewat.
- Meja terpakai, pembayaran lunas, dan dapur belum selesai tidak disatukan sebagai satu status.
- Hasil timeout diperiksa memakai operasi sama; tidak ada tindakan bayar baru ketika hasil lama belum pasti.
- Logout tidak menjanjikan pemulihan draft: cache sensitif dibersihkan sesuai arsitektur. Refund digital tetap pencatatan manual, bukan klaim pengiriman uang otomatis.
- Kontrak yang belum lengkap dicatat sebagai dependensi sebelum kontrol UI dibuat, bukan diisi endpoint rekaan.
- Layout ponsel/tablet/desktop, keyboard, fokus, sentuhan dan state error ditulis sebagai kriteria implementasi.

## Pemeriksaan kontras token

Metode: luminance sRGB linear dan `(Lterang + 0,05) / (Lgelap + 0,05)`, dihitung menggunakan PowerShell/.NET. Ambang teks normal 4,5:1; batas/fokus kontrol 3:1. Angka dibulatkan dua desimal setelah perbandingan threshold.

| Pasangan foreground / background | Rasio | Hasil |
| --- | --- | --- |
| text / surface | 14,33:1 | PASS teks |
| text / canvas | 13,14:1 | PASS teks |
| text-muted / surface | 6,19:1 | PASS teks |
| text-muted / canvas | 5,68:1 | PASS teks |
| putih / primary | 8,08:1 | PASS teks |
| putih / primary-hover | 10,86:1 | PASS teks |
| danger / surface | 7,45:1 | PASS teks |
| warning / surface | 6,99:1 | PASS teks |
| control-border / surface | 4,12:1 | PASS kontrol |
| control-border / canvas | 3,78:1 | PASS kontrol |
| focus / canvas | 7,41:1 | PASS fokus |

Divider dekoratif tidak digunakan sebagai satu-satunya batas kontrol. Opacity, warna disabled, status latar tambahan, ilustrasi/foto dan palet aktual saat implementasi tetap perlu diuji ulang.

## Gate penyerahan dokumen

- PASS arah desain: tiga jawaban pemilik dicatat pada DESIGN.md; nilai turunan diberi alasan.
- PASS tujuan layout: wireframe/komponen dipetakan ke pekerjaan kasir, meja, dapur, pembayaran, shift, koreksi dan laporan.
- PASS spesifikasi state: loading/kosong/error, akses berakhir, konflik, hasil ambigu, offline/reconnect, cetak, dan pembaruan terdokumentasi.
- PASS kontras token: 11 pasangan di tabel dihitung di atas ambang yang dinyatakan.
- Pemeriksaan runtime belum berlaku: belum ada source UI yang dapat dibangun/diklik. Tidak mengklaim bebas overflow, seluruh tombol berjalan, pembaca layar lolos, atau seluruh state sudah teruji.

## Batas dan tindak lanjut

Pemeriksaan penyerahan lokal: 29 dokumen Markdown, 123 tautan lokal valid, 8 ID alur UX unik, 4 wireframe teks dengan fence lengkap, dan state pemulihan utama tersedia. Whitespace serta penamaan diperiksa. Hash dan kecocokan remote diverifikasi terpisah setelah commit/push.

Wireframe teks tidak menggantikan uji visual pada viewport nyata. Uji minimal 360/768/1024/1280px, tinggi landscape pendek, zoom 200%, keyboard virtual, fokus dialog, offline, dan Android sentuh setelah implementasi tersedia. O-001–05, konfigurasi outlet, presisi data, model printer, runtime/dependency dan lisensi tetap terbuka; review visual tidak menyetujuinya otomatis.
