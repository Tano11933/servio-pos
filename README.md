# Servio POS

POS untuk operasional kafe dan restoran, dikembangkan bertahap menuju pilot satu outlet dengan target dua bulan.

## Status saat ini

Fondasi dokumentasi dan workflow pengembangan tersedia. Draft PRD, matriks akses, dan rancangan validasi telah disusun untuk review. Berikutnya adalah aturan bisnis dan transisi status. Aplikasi, API, database, dan pipeline belum diimplementasikan; belum ada perintah untuk menjalankan aplikasi.

Repository: [Tano11933/servio-pos](https://github.com/Tano11933/servio-pos). Branch utama: `main`. Identitas visual dan lisensi distribusi belum ditentukan.

## Target pilot

- Pesanan takeaway dan dine-in dengan tagihan terbuka.
- Menu, varian, modifier, resep, bahan, konversi satuan, stok, dan waste.
- Antrean dapur serta perubahan status persiapan.
- Pembayaran manual tunai, QRIS, dan EDC.
- Shift kasir, pembatalan, retur, laporan, dan hak akses.
- Web responsif, PWA Android, dan satu model printer thermal yang telah diuji.
- Isolasi data antarusaha, pengulangan request yang aman, pemulihan gangguan, dan backup yang diuji restore.

Pilot menggunakan satu outlet, satu mata uang, serta pembayaran penuh per tagihan. Struktur data dirancang untuk SaaS dengan tenant sebagai usaha dan outlet sebagai lokasi.

## Stack yang disepakati

| Bagian | Teknologi |
| --- | --- |
| Frontend | React, Vite, TypeScript |
| Backend | NestJS, TypeScript, REST/OpenAPI |
| Database | PostgreSQL |
| Akses data | Prisma dan SQL eksplisit untuk operasi kritis |
| Data frontend | TanStack Query |
| State lokal | State React/Zustand sesuai kebutuhan |
| Arsitektur | Modular monolith |
| Pengujian terencana | Jest/Vitest, Playwright, dan pengujian integrasi PostgreSQL |

Versi runtime dan dependency akan dipilih serta dikunci saat kerangka aplikasi dibuat. Redis, queue terpisah, dan layanan berbayar bukan prasyarat fondasi proyek.

Git tersedia; command Node/npm terdeteksi tetapi Node belum memiliki versi aktif pada pengelola versinya. Konfigurasi runtime perlu diselesaikan sebelum menjalankan scaffold.

## Batas versi pertama

Tidak termasuk offline penuh, split bill, pembayaran sebagian, gabung/pindah meja, transfer stok antaroutlet, costing/HPP lengkap, pembelian pemasok lengkap, pembayaran digital otomatis, integrasi ERP/MRP, serta billing langganan SaaS.

PWA tidak menjanjikan transaksi offline atau kompatibilitas semua printer. QRIS/EDC pada pilot dikonfirmasi manual oleh kasir.

## Pola pengerjaan

Pengembangan mengikuti milestone dan branch per fitur. Setiap perubahan mencakup hasil yang dapat direview, validasi yang relevan, serta pembaruan catatan progres.

Target commit adalah 3–4 perubahan bermakna per hari. Jumlah tersebut tidak dipenuhi melalui commit kosong, pengubahan tanggal, atau pemecahan perubahan secara artifisial.

## Dokumentasi

- [Indeks dokumen dan statusnya](docs/README.md)
- [Product brief dan cakupan pilot](docs/product/brief.md)
- [PRD dan kriteria penerimaan](docs/product/prd.md)
- [Usulan matriks akses](docs/product/access-matrix.md)
- [Rancangan validasi](docs/product/acceptance-matrix.md)
- [Roadmap milestone](docs/roadmap.md)
- [Daftar keputusan dan hal yang perlu dikunci](docs/decisions.md)
- [Backlog sesi berikutnya](docs/backlog.md)
- [Progres dan handoff](docs/progress.md)
- [Panduan kontribusi](CONTRIBUTING.md)
- [Panduan pengembangan](AGENTS.md)

## Validasi fondasi

Pada tahap dokumentasi, jalankan `git diff --check` untuk memeriksa whitespace dan periksa tujuan tautan Markdown lokal. Pemeriksaan aplikasi akan ditambahkan bersama kode yang bisa dijalankan. Status dokumentasi tidak menjadi bukti kesiapan pilot.
