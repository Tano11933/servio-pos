# Panduan pengembangan

## Konteks dan sumber kebenaran

Servio POS adalah POS SaaS F&B menuju pilot satu outlet dengan target dua bulan. Stack yang disepakati: React/Vite/TypeScript, NestJS, PostgreSQL, Prisma, REST/OpenAPI, modular monolith. Scope berada di [product brief](docs/product/brief.md); milestone berada di [roadmap](docs/roadmap.md).

Sebelum mengubah kode atau dokumen:

1. Baca [progres terakhir](docs/progress.md), [backlog](docs/backlog.md), dan [daftar keputusan](docs/decisions.md).
2. Periksa status Git dan perubahan pengguna. Jangan menimpa atau membatalkan perubahan yang tidak dibuat dalam sesi ini.
3. Tentukan pekerjaan aktif dari milestone dan backlog, bukan dari tanggal kalender atau jumlah commit. Jangan menambahkan label hari pada dokumen, branch, atau commit.
4. Baca PRD/aturan bisnis/ADR/kontrak terkait apabila sudah tersedia. [Indeks dokumen](docs/README.md) membedakan dokumen tersedia dari yang direncanakan.
5. Selesaikan pekerjaan aktif sampai dapat direview dan divalidasi. Ikuti dependensi milestone dan checkpoint; instruksi pengguna terbaru dapat mengubah target sesi.

## Status repository

Repository saat ini berisi dokumentasi, belum ada source aplikasi, package manifest, lockfile, database, atau CI aplikasi. Jangan mengklaim `npm test`, build, atau endpoint telah tersedia. Ketika scaffold dibuat, tambahkan perintah yang benar-benar telah dijalankan ke README dan panduan ini.

## Cara bekerja

- Dokumen dan laporan pengguna menggunakan Bahasa Indonesia. Identifier kode serta commit menggunakan Bahasa Inggris.
- Pilih branch `feat/*`, `fix/*`, `docs/*`, atau `chore/*` sesuai [panduan kontribusi](CONTRIBUTING.md). Branch harus mengikuti perubahan yang dapat direview.
- Target 3–4 commit bermakna per sesi harian. Jangan membuat commit kosong, mengubah tanggal, atau memecah perubahan yang tidak masuk akal.
- Jika Git gagal karena sandbox, gunakan alur izin yang tersedia; jangan memindahkan metadata Git untuk menghindari pembatasan.
- Merge ke `main` setelah review dan pemeriksaan. Review mandiri tidak menggantikan persetujuan pemilik proyek.
- Commit menggunakan identitas Git pemilik repository tanpa atribusi tambahan.
- Push hanya ke remote pengguna yang sudah diketahui. Tanpa remote, laporkan commit/branch lokal dan hambatannya; jangan menciptakan URL, PR, atau status push.
- Jangan menambahkan layanan berbayar, dependency, atau fitur di luar scope tanpa kebutuhan nyata. Catat keputusan teknis yang berdampak pada ADR setelah struktur ADR tersedia.
- Jangan menulis secret, data pelanggan, atau dump database ke Git. `.env` lokal diabaikan; contoh konfigurasi berisi placeholder.

## Kontrak dan aturan transaksi

- Definisikan kontrak serta acceptance criteria sebelum fitur terkait diimplementasikan. Jangan menganggap keputusan terbuka sebagai keputusan final.
- Backend menjadi penghitung final harga, diskon, pajak, service charge, dan total. Gunakan nilai uang eksak; jangan menghitung uang dengan floating point biner biasa.
- Setiap query/command/resource terkait bisnis harus memverifikasi tenant/outlet dan izin di server. Uji akses lintas tenant melalui API langsung, termasuk relasi, ekspor, file, dan proses background.
- Pisahkan siklus hidup pesanan, pembayaran, dan dapur. Efek stok/kas mengikuti transisi yang terdokumentasi, bukan efek samping UI.
- Pembayaran manual bukan integrasi payment gateway. Jangan menampilkan konfirmasi otomatis yang belum tersedia.
- Operasi kritis harus memiliki idempotency di server dan constraint database. Tombol yang dinonaktifkan bukan mekanisme deduplikasi.
- Pesanan bersamaan, perubahan versi, reservasi/konsumsi bahan, dan retur bersamaan memerlukan kontrol database serta pengujian integrasi nyata.
- Perubahan bisnis yang harus atomik berada dalam transaksi PostgreSQL. Jangan menjadikan cache atau state browser sebagai sumber kebenaran.
- Simpan snapshot harga/resep yang diperlukan dan ledger/audit agar perubahan katalog tidak mengubah riwayat transaksi.
- Pembatalan/retur mempertahankan transaksi asal. Refund makanan tidak otomatis mengembalikan bahan resep ke stok.
- PWA tetap online untuk pengesahan transaksi. Setelah timeout, periksa hasil operasi dengan identitas yang sama sebelum mencoba ulang.
- Gagal cetak/cetak ulang tidak membuat penjualan baru. Nyatakan dukungan hanya untuk kombinasi perangkat yang diuji.

## UX dan struktur aplikasi

- Baca [DESIGN.md](DESIGN.md), [spesifikasi UX](docs/design/ux-spec.md), dan [state interaksi](docs/design/interaction-states.md) sebelum UI. Arah pemilik: terang/hangat bernuansa kafe, aksen hijau, tablet Android landscape. Token turunan menjadi baseline yang perlu diverifikasi pada UI nyata.
- UI kasir harus mendukung keyboard, fokus terlihat, perangkat sentuh, serta keadaan kosong/loading/error/koneksi terputus.
- Saat source tersedia, jaga batas modul bisnis; jangan menaruh kalkulasi uang dan otorisasi hanya di frontend.
- Runtime, versi dependency, struktur workspace, dan detail sesi diputuskan saat kontrak arsitektur/scaffold dibuat. Jangan menambahkan file setup spekulatif yang tidak dapat diuji.

## Validasi dan handoff

- Pada perubahan dokumentasi, jalankan `git diff --check` dan periksa tujuan tautan lokal. Tahap fondasi belum memiliki pengujian aplikasi.
- Pada kode, jalankan pemeriksaan yang relevan terhadap perubahan. Uji transaksi terhadap PostgreSQL untuk klaim constraint, locking, rollback, atau isolasi.
- Jangan mengklaim validasi printer, staging, restore, atau penggunaan outlet tanpa bukti pelaksanaan.
- Sebelum menyerahkan hasil, periksa diff/staged files agar tidak memasukkan artefak lokal atau perubahan di luar tugas.
- Perbarui status roadmap, backlog, dan progres: hasil nyata, perintah/hasil pemeriksaan, penghambat, serta langkah berikutnya. Bedakan selesai implementasi, menunggu review, dan selesai review.
- Laporkan hasil, cara mereview, validasi, dan batasan secara ringkas. Readiness pilot membutuhkan pemeriksaan operasional dan keandalan, bukan sekadar roadmap selesai.
