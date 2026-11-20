# Backlog aktif

Milestone aktif: fondasi produk dan pengembangan. Pekerjaan mengikuti fitur dan dependensi; tanpa label hari pada dokumen, branch, atau commit.

## Dokumen kebutuhan tersedia

Asal branch: `docs/product-requirements`, diintegrasikan ke `main` dengan persetujuan pemilik. Penyusunan dokumen selesai; detail izin dan aturan yang ditandai usulan tetap menunggu keputusan. Belum ada implementasi fitur.

| ID | Hasil | Status |
| --- | --- | --- |
| PRD-01 | PRD dengan ID, aktor, prioritas, dan acceptance criteria | Draft tersedia |
| PRD-02 | Matriks peran serta batas tenant/outlet | Usulan menunggu review |
| PRD-03 | Skenario takeaway, dine-in, dapur, pembayaran, shift, koreksi, dan gangguan | Tertulis di PRD |
| PRD-04 | Pemetaan semua requirement ke skenario validasi | Rancangan tersedia; belum dijalankan |
| PRD-05 | Batas pilot dan keputusan terbuka | Tercatat; tidak memperluas scope |

## Berikutnya: aturan bisnis

Branch terencana: `docs/business-rules`. Mulai dari `main` terbaru yang sudah memuat dokumen kebutuhan.

| ID | Tugas | Kriteria selesai |
| --- | --- | --- |
| BIZ-01 | State machine pesanan, pembayaran, dapur, dan meja | Transisi, prasyarat, peran, serta penolakan tertulis; pesanan dan persiapan tidak dicampur dengan status pembayaran |
| BIZ-02 | Reservasi/konsumsi bahan dan pembatalan | Waktu penguncian resep, stok terbatas, pelepasan reservasi, dan waste jelas |
| BIZ-03 | Uang, diskon, pajak, service charge, pembulatan | Urutan hitung dan contoh expected result eksak; konfigurasi outlet dikonfirmasi |
| BIZ-04 | Shift, retur, dan refund manual | Batas kumulatif, atribusi kas, refund setelah shift asal ditutup, serta operasi bersamaan jelas |
| BIZ-05 | Review matriks izin dan ketergantungan | Keputusan pemilik dibedakan dari usulan; PRD/keputusan/validasi diperbarui bila berubah |

## Antrean berikutnya

1. Arsitektur, ERD, ADR, dan kontrak API sebelum fitur terkait.
2. Arah visual pada `DESIGN.md` di root dan UX/wireframe pada `docs/design/ux-spec.md`; belum dibuat atau disetujui.
3. Runtime/dependency, scaffold aplikasi/database, CI, serta kajian kandidat printer.

Command Node/npm sebelumnya ditemukan tanpa versi Node aktif. Verifikasi kembali dan pilih runtime yang didukung sebelum scaffold.

## Kebutuhan eksternal

- Remote: `https://github.com/Tano11933/servio-pos.git`.
- Printer fisik, hosting, dan akses outlet untuk validasi operasional.
- Lisensi distribusi belum ditentukan.
