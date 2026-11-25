# ADR-001: modular monolith dan batas kepercayaan

Status: baseline rancangan dokumentasi; belum terimplementasi. Mengikuti stack yang disepakati dan tetap kondisional terhadap keputusan produk terbuka.

## Konteks

Pilot membutuhkan transaksi lintas pesanan, bahan, pembayaran dan kas. Memecahnya ke layanan terpisah menambah kegagalan terdistribusi sebelum ada kebutuhan skala yang terukur.

## Keputusan

- Satu NestJS modular monolith dan satu PostgreSQL; batas ownership modul mengikuti [overview](../overview.md).
- React/Vite/PWA memakai REST satu origin. Tidak ada database lokal yang mengesahkan transaksi offline.
- Sesi opaque server-side, cookie aman, CSRF/Origin, serta pemeriksaan membership/outlet pada request dan transaksi kritis.
- Tenant/outlet eksplisit pada query dan FK komposit. RLS dapat ditambahkan kemudian sebagai lapisan tambahan setelah role/konteks koneksi diuji; tidak diklaim tersedia pada baseline.
- Kitchen queue memakai polling snapshot. Tanpa Redis, broker, atau cache sebagai prasyarat pembayaran.
- Workspace yang direncanakan: `apps/web`, `apps/api`, `packages/contracts`. Shared contracts berisi DTO/schema, bukan akses database, secret, atau aturan otorisasi frontend.

## Konsekuensi dan alternatif

Deployment awal lebih sederhana dan mutasi bisnis memakai satu transaksi. API/database tetap titik kritis yang memerlukan backup dan observabilitas. Multi-instance dapat ditambahkan dengan sesi/operasi di database; polling tidak bergantung memori proses. Microservice, JWT browser panjang umur, RLS tanpa uji konteks, dan offline-first tidak dipilih untuk pilot.

Runtime/dependency/provider hosting diputuskan saat scaffold. Kontrak operasional dan backup perlu dibuktikan pada staging, bukan dinyatakan selesai oleh ADR.
