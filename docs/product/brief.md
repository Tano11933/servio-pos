# Product brief Servio POS

Status: baseline cakupan pilot. Dokumen ini belum merupakan PRD atau bukti bahwa fitur telah diimplementasikan.

## Tujuan

Membantu kafe/restoran mencatat pesanan, pembayaran, penggunaan bahan, dan kas operasional secara konsisten. Proyek menjadi produk yang dapat diuji di usaha nyata sekaligus portfolio engineering dengan riwayat pengembangan yang dapat diperiksa.

## Pengguna

| Peran | Kebutuhan utama |
| --- | --- |
| Owner | Mengatur usaha/outlet, pengguna, katalog, konfigurasi, dan membaca laporan |
| Manager | Mengawasi operasi, stok, koreksi, serta tindakan sensitif sesuai izin |
| Kasir | Mencatat pesanan, menerima pembayaran, mencetak struk, serta membuka/menutup shift |
| Dapur | Melihat antrean pesanan dan memperbarui status persiapan |

Matriks izin rinci ditetapkan di PRD. Menyembunyikan tombol tidak menggantikan pemeriksaan izin di backend.

## Masalah yang diselesaikan

- Pesanan takeaway dan meja perlu tersambung dengan dapur serta pembayaran.
- Perubahan stok bahan perlu memiliki asal yang dapat ditelusuri.
- Respons timeout dan pengulangan request tidak boleh menghasilkan pembayaran/penjualan ganda.
- Pembatalan dan retur perlu mempertahankan riwayat serta dampak kas/stok yang benar.
- Pemilik perlu merekonsiliasi penjualan, metode pembayaran, dan kas shift.
- Gangguan layanan perlu memiliki prosedur pemulihan, termasuk restore database.

## Batas pilot

| Area | Termasuk |
| --- | --- |
| Usaha | Pilot satu outlet; model data dan akses mengenali tenant dan outlet |
| Pesanan | Takeaway, dine-in, tagihan terbuka, tambahan item, catatan |
| Katalog | Kategori, menu, varian, modifier, harga, ketersediaan |
| Inventori | Bahan, resep, konversi satuan, stok awal/penerimaan, penyesuaian, waste, riwayat |
| Dapur | Antrean sederhana, pengiriman batch, dan status persiapan |
| Pembayaran | Penuh per tagihan, manual tunai/QRIS/EDC, kembalian, struk |
| Kontrol operasional | Shift, kas masuk/keluar, selisih, pembatalan, retur, audit |
| Laporan | Penjualan, pembayaran, menu, shift, stok, waste, dan CSV |
| Platform | Browser responsif, PWA Android, satu kombinasi perangkat/printer thermal teruji |
| Keandalan | Isolasi tenant, idempotency, transaksi atomik, konflik perubahan, pemulihan, backup/restore |

## Di luar pilot

Offline penuh; split bill; pembayaran sebagian; gabung/pindah meja; transfer stok antaroutlet; costing/HPP lengkap; pembelian pemasok lengkap; integrasi payment gateway; ERP/MRP; billing SaaS; onboarding usaha mandiri; dukungan universal printer.

## Indikator penerimaan

Kriteria rinci akan menggunakan ID di PRD. Baseline penerimaan pilot:

1. Pesanan takeaway dan dine-in dapat ditelusuri dari kasir ke dapur, pembayaran, dan pergerakan bahan.
2. Total dihitung final di server memakai representasi uang eksak dan aturan pembulatan terdokumentasi.
3. Request berulang serta operasi bersamaan tidak menggandakan pembayaran atau mengurangi stok secara tidak sah.
4. Pengguna tidak dapat mengakses data atau operasi tenant/outlet di luar izinnya, termasuk melalui API langsung.
5. Pembatalan/retur memiliki alasan, otorisasi, jejak audit, dan nilai yang dapat direkonsiliasi.
6. Penjualan, retur, dan mutasi kas menjelaskan saldo kas yang diharapkan pada penutupan shift.
7. Putus koneksi dapat ditangani dengan pemulihan draft dan pengecekan hasil operasi; penjualan baru tidak disahkan offline.
8. Cetak ulang tidak membuat transaksi baru; kompatibilitas dicatat untuk perangkat yang benar-benar diuji.
9. Restore backup ke lingkungan terpisah berhasil diverifikasi sebelum pilot.

## Batas waktu dan tanggung jawab

Target adalah dua bulan; pemilik menyediakan 1–2 jam per hari untuk review dan penggunaan. Pengembangan mengikuti milestone dan mencatat hasil validasi. Pemilik mengunci keputusan produk, menyediakan printer, serta mencoba operasi outlet.

Pilot dimulai setelah pemeriksaan kesiapan lolos. Jika masih ada risiko kritis pada target rilis, hasil diberi status kandidat pilot beserta penghambatnya, bukan diklaim siap produksi.
