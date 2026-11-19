# Progres proyek

## Status terbaru

| Item | Status |
| --- | --- |
| Produk | Servio POS |
| Milestone aktif | Fondasi produk dan pengembangan |
| Selesai | Fondasi repository, scope pilot, roadmap milestone, dan workflow review |
| Berikutnya | PRD, matriks akses, dan kriteria penerimaan |
| Repository | [Tano11933/servio-pos](https://github.com/Tano11933/servio-pos) |

## Hasil fondasi

- Git lokal, aturan ignore, line ending, konfigurasi editor, dan README tersedia.
- Product brief, roadmap, keputusan, dan backlog menyimpan kebutuhan yang disepakati.
- Panduan pengembangan/kontribusi serta template issue/PR tersedia.
- Roadmap mengikuti milestone tanpa penomoran hari; branch dan commit mengikuti perubahan.
- Dokumen yang belum dibuat tercatat dalam indeks, bukan disajikan sebagai spesifikasi selesai.

## Validasi

- Whitespace diperiksa dengan `git diff --check`.
- Tujuan tautan Markdown lokal diperiksa terhadap file tersedia.
- Template issue memiliki metadata `name`, `about`, dan `title`.
- Ignore mengecualikan konfigurasi lokal/backup/dependency/hasil build; contoh environment tetap dapat dilacak.
- Identitas Git memakai konfigurasi pemilik repository; riwayat dapat diperiksa dengan `git log --format=full`.

## Batasan

- Belum ada source aplikasi, database, dependency, build/test aplikasi, atau deployment.
- Command Node/npm terdeteksi tetapi belum ada versi Node aktif; selesaikan sebelum scaffold.
- Printer, restore database, dan penggunaan outlet belum diuji.
- Palet, tipografi, dan lisensi distribusi belum ditentukan.

## Langkah berikutnya

1. Kerjakan PRD-01 sampai PRD-05 pada backlog.
2. Kunci aturan status, stok, harga, pembulatan, retur, dan shift.
3. Susun arsitektur/data model, design brief, dan scaffold sesuai dependensi.

Status push serta hash akhir diperiksa melalui Git; URL repository bukan bukti bahwa perubahan telah dipush.
