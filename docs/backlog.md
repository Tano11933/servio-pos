# Backlog aktif

Milestone: fondasi produk dan pengembangan. Gunakan fitur/milestone tanpa label hari pada dokumen, branch, atau commit. Identitas commit memakai pemilik repository tanpa atribusi tambahan.

## Hasil terkini

Dokumen produk dan arsitektur tersedia. Baseline desain dibuat melalui `docs/product-design` dengan otorisasi review dan merge setelah validasi. Ini belum merupakan UI aplikasi.

| Tugas | Hasil | Status |
| --- | --- | --- |
| UX-PLAN-01 | DESIGN.md root: terang/hangat, aksen hijau, tablet Android landscape | Arah disetujui pemilik; token turunan terdokumentasi |
| UX-PLAN-02 | Navigasi dan wireframe kasir, meja, dapur, pembayaran, koreksi, shift | Spesifikasi tersedia |
| UX-PLAN-03 | State error/conflict/reconnect, draft, PWA dan aksesibilitas | Spesifikasi tersedia; runtime belum diuji |
| UX-PLAN-04 | Review izin/kontrak dan 11 pasangan kontras token | Review dokumen selesai; gap API dicatat |

## Berikutnya: prasyarat dan scaffold

Branch terencana `chore/application-scaffold`, dari main terbaru setelah desain terintegrasi.

| ID | Pekerjaan | Kriteria selesai |
| --- | --- | --- |
| SETUP-01 | Periksa environment dan pilih versi | Node aktif, package manager, PostgreSQL dan versi dependency kompatibel diverifikasi; ADR serta lockfile dipersiapkan |
| SETUP-02 | Kerangka workspace | apps/web, apps/api, packages/contracts sesuai arsitektur; script dev/build/typecheck/lint terdokumentasi dan benar-benar dijalankan |
| SETUP-03 | Konfigurasi lokal dan health | Environment example tanpa secret, DB lokal terisolasi, health live/ready, error aman dan migrasi awal sesuai kontrak yang dikunci |
| SETUP-04 | Pemeriksaan otomatis dan handoff | CI menjalankan pemeriksaan yang tersedia, README/AGENTS/progres mencatat perintah serta batasannya |

Sebelum schema bisnis: putuskan O-001–05, presisi kuantitas/rate, mata uang dan konfigurasi outlet; jangan menganggap merge dokumen menyetujui aturan bisnis. Lengkapi request/response endpoint yang ditandai gap pada UX sebelum kontrol terkait dibuat. UI final mengikuti DESIGN.md dan harus diuji reflow, fokus serta perilakunya.

## Kebutuhan eksternal

Remote: `https://github.com/Tano11933/servio-pos.git`. Printer fisik, hosting, akses outlet, lisensi, RPO/RTO dan target beban tetap diperlukan pada tahap terkait. Node sebelumnya belum memiliki versi aktif; verifikasi ulang sebelum scaffold.
