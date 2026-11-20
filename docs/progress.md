# Progres proyek

## Status terbaru

| Item | Status |
| --- | --- |
| Produk | Servio POS |
| Milestone aktif | Fondasi produk dan pengembangan |
| Hasil terbaru | Draft PRD, usulan matriks akses, dan rancangan validasi |
| Review | Menunggu review pemilik; izin dan aturan bisnis belum dikunci |
| Branch | `docs/product-requirements` |
| Berikutnya | Aturan bisnis dan transisi status |
| Repository | [Tano11933/servio-pos](https://github.com/Tano11933/servio-pos) |

## Catatan kerja tetap

Gunakan milestone dan nama fitur, tanpa label hari pada dokumen, branch, atau commit. Commit memakai identitas pemilik repository tanpa atribusi tambahan. Jumlah commit mengikuti perubahan bermakna.

## Hasil

- Fondasi Git, README, roadmap, panduan pengembangan, serta template issue/PR tersedia.
- [PRD](product/prd.md) menetapkan kebutuhan wajib, kriteria penerimaan, alur operasional, dan batas pilot.
- [Matriks akses](product/access-matrix.md) memisahkan usulan hak owner/manager/kasir/dapur serta batas tenant/outlet.
- [Matriks validasi](product/acceptance-matrix.md) memetakan setiap requirement ke skenario serta bukti yang perlu dikumpulkan; seluruh skenario aplikasi belum dijalankan.
- Daftar keputusan tetap menandai izin, hitungan uang, konsumsi bahan, status, dan retur sebagai keputusan terbuka.
- Lokasi arah visual diselaraskan ke `DESIGN.md` pada root; dokumennya belum dibuat.

## Validasi dokumen

Pemeriksaan mencakup whitespace, tujuan tautan lokal, ID requirement unik, cakupan requirement dalam matriks validasi, metadata template, label/atribusi yang tidak diinginkan, serta metadata commit. Validasi dokumen tidak menjadi bukti aplikasi sudah bekerja.

## Batasan

- Belum ada source aplikasi, database, dependency, build/test aplikasi, atau deployment.
- Node sebelumnya belum memiliki versi aktif; periksa sebelum scaffold.
- Printer, restore database, dan penggunaan outlet belum diuji.
- Palet, tipografi, dan lisensi belum ditentukan.
- Hasil branch kebutuhan belum di-merge ke `main`; review pemilik tetap diperlukan.

## Langkah berikutnya

Review PRD dan usulan izin, lalu kerjakan BIZ-01 sampai BIZ-05 pada backlog. Lanjutkan arsitektur/data model dan desain sesuai dependensi. Status push dan hash akhir diperiksa melalui Git.
