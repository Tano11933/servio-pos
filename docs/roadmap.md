# Roadmap Servio POS

Target: pilot satu outlet dalam dua bulan. Pekerjaan mengikuti milestone dan dependensi fitur. Review dilakukan berkala berdasarkan hasil yang dapat dicoba, tanpa penomoran hari.

Status tugas: `berikutnya`, `belum dimulai`, `berjalan`, `selesai`, atau `terhambat`. [Progres](progress.md) menyimpan hasil aktual; [backlog](backlog.md) merinci pekerjaan yang siap dikerjakan.

## Fondasi produk dan pengembangan

Status: berjalan. Kebutuhan, aturan bisnis, arsitektur/model data, ADR, REST awal, strategi pengujian, dan baseline desain/UX tersedia. Arah pemilik sudah dicatat; izin serta pilihan operasional tetap terbuka. Scaffold menyusul; milestone belum selesai karena aplikasi belum dapat dijalankan.

- Git, README, workflow branch, panduan pengembangan, dan catatan progres.
- PRD, matriks hak akses, dan kriteria penerimaan pilot.
- Alur bisnis, status pesanan/pembayaran/dapur, dan aturan konsumsi bahan.
- Arsitektur modular monolith, ERD, dan ADR.
- Wireframe, design brief yang disetujui, dan kerangka UI.
- Pemilihan runtime/dependency, scaffold frontend/backend, database, dan CI.
- Kajian kandidat printer, koneksi, dan jalur cetak.

Hasil: kebutuhan serta kontrak dasar jelas dan aplikasi awal dapat dijalankan lokal. Milestone belum selesai sebelum scaffold berhasil dijalankan.

## Akses dan katalog

Status: belum dimulai.

- Autentikasi, sesi, tenant, outlet, dan membership.
- Peran, pemeriksaan izin server, dan pengujian isolasi tenant/outlet.
- Kategori, menu, varian, modifier, harga, dan ketersediaan.
- Pencarian menu dan layout kasir responsif.
- Review login sampai pengelolaan katalog.

Checkpoint: pengguna dapat mengelola katalog sesuai izin; akses lintas tenant ditolak melalui API langsung.

## Inventori F&B

Status: belum dimulai.

- Bahan, satuan dasar, konversi dalam dimensi sama, dan validasi kuantitas.
- Resep menu/modifier dan kebutuhan bahan per porsi.
- Ledger stok, stok awal, penerimaan, dan saldo.
- Penyesuaian dengan alasan, waste, serta riwayat.
- Pengujian perhitungan bahan dan perubahan stok bersamaan.
- Review dengan contoh resep nyata.

Hasil: kebutuhan bahan dapat dihitung dan perubahan stok memiliki sumber yang dapat ditelusuri.

## Pesanan dan dapur

Status: belum dimulai.

- Keranjang takeaway dengan snapshot harga/resep.
- Meja, tagihan dine-in terbuka, dan tambahan item.
- Perlindungan konflik perubahan dari beberapa perangkat.
- Pengiriman batch dan integrasi reservasi/konsumsi bahan.
- Layar dapur, status persiapan, pembaruan antrean, dan penyambungan ulang.
- Pengujian serta review kasir–dapur–stok.

Checkpoint: kedua alur tersambung ke dapur; konflik pesanan dan perebutan bahan memiliki bukti pengujian.

## Pembayaran dan shift

Status: belum dimulai.

- Hitungan server untuk diskon, pajak, service charge, dan pembulatan.
- Pembayaran manual tunai/QRIS/EDC dan kembalian tunai.
- Idempotency, pembayaran bersamaan, dan pengecekan hasil setelah timeout.
- Struk digital dan riwayat transaksi.
- Buka shift, saldo awal, dan kas masuk/keluar.
- Tutup shift, hitungan aktual, selisih, dan pengujian rekonsiliasi.
- Review pesanan sampai penutupan shift.

Hasil: pembayaran penuh diselesaikan sekali dan direkonsiliasi dengan kas shift.

## Pembatalan, retur, dan laporan

Status: belum dimulai.

- Pembatalan dengan alasan, izin, dan dampak ke dapur/reservasi/waste.
- Retur terkait transaksi asal dan pengembalian dana manual.
- Batas kuantitas/nilai retur, audit, dan pencegahan proses berulang.
- Laporan penjualan, metode pembayaran, dan menu.
- Laporan shift, saldo/pemakaian bahan, waste, stok minimum, dan CSV.
- Rekonsiliasi laporan terhadap transaksi sumber.

Checkpoint: operasi inti lengkap; koreksi dan angka laporan dapat ditelusuri. Retur makanan tidak otomatis mengembalikan bahan resep ke stok.

## Perangkat dan staging

Status: belum dimulai.

- PWA, instalasi Android, caching aset, dan pembaruan versi.
- Pemulihan draft lokal dengan pemisahan pengguna/outlet.
- Indikator koneksi dan pengecekan status transaksi setelah gangguan.
- Adapter cetak serta format struk untuk satu model thermal terpilih.
- Cetak fisik, gagal cetak, dan cetak ulang tanpa transaksi baru.
- Container produksi, staging, dan pipeline deployment.
- Log, error tracking, health check, dan prosedur migrasi.
- Review Android dan printer sasaran.

Hasil: aplikasi dapat dicoba melalui staging. Kandidat printer dikaji sejak fondasi; perangkat fisik tersedia sebelum integrasi. Dukungan dinyatakan berdasarkan hasil uji.

## Kesiapan pilot

Status: belum dimulai.

- Regresi izin dan isolasi tenant, termasuk ekspor/proses background.
- Checkout bersamaan, retry, rollback, dan stok terbatas.
- Target RPO/RTO, backup, retensi, serta restore ke lingkungan terpisah.
- Simulasi restart, timeout, dan kegagalan proses.
- Ukur respons dengan perangkat, data, dan beban outlet yang disepakati.
- Perbaiki temuan prioritas.
- Onboarding menu/bahan/resep/meja/pengguna, panduan, dan latihan operasional.

Checkpoint: bukti pengujian tersedia, restore berhasil, data outlet siap, dan tidak ada masalah kritis terhadap data, hitungan, atau akses.

## Pilot dan rilis

Status: belum dimulai.

- Pilot terbatas setelah lolos pemeriksaan kesiapan.
- Pemeriksaan transaksi, stok, pembayaran, dan shift dari penggunaan nyata.
- Perbaikan prioritas serta pengujian ulang.
- Demo dan studi kasus portfolio berdasarkan hasil aktual.
- Release notes, tag bila lolos, dan backlog berikutnya.

Hasil: rilis pilot dengan bukti penggunaan dan batas dukungan. Jika belum lolos kesiapan, serahkan kandidat pilot beserta penghambat; jangan mengklaim siap produksi.

## Aturan penyesuaian

- Selesaikan kekurangan checkpoint sebelum memperluas fitur.
- Tugas selesai memiliki acceptance criteria dan bukti validasi.
- Gunakan branch per perubahan dan commit bermakna; jangan membuat aktivitas semu.
- Scope di luar pilot ditunda kecuali kebutuhan diubah secara eksplisit.
- Sesudah pilot, lanjutkan feedback, prioritas, implementasi, pengujian, dan rilis berkala.
