# Progres proyek

## Status terbaru

| Item | Status |
| --- | --- |
| Produk | Servio POS |
| Milestone | Fondasi produk dan pengembangan |
| Hasil terbaru | Arsitektur, ERD/kamus data, dua ADR, REST awal, strategi pengujian dan review dokumen |
| Integrasi | Pemilik mengotorisasi review dan merge ke main setelah validasi; hash/status remote diverifikasi lewat Git |
| Asal branch | docs/system-architecture |
| Berikutnya | Design brief bersama pemilik dan spesifikasi UX |
| Repository | [Tano11933/servio-pos](https://github.com/Tano11933/servio-pos) |

## Catatan kerja tetap

Penamaan mengalir berdasarkan fitur/milestone, tanpa label hari pada dokumen, branch, atau commit. Metadata memakai identitas Git pemilik tanpa atribusi tambahan. Jangan membuat commit kosong untuk memenuhi jumlah.

## Hasil

- PRD dan aturan bisnis sudah diintegrasikan pada main; pilihan yang ditandai usulan tetap terbuka.
- [Overview](architecture/overview.md) menjelaskan container, ownership modul, sesi/CSRF, tenant dan alur kritis.
- [ERD/kamus data](architecture/data-model.md) memetakan snapshot, unit refund, saldo/ledger, scope FK dan constraint.
- ADR struktur serta transaksi menetapkan baseline sesi, polling, locking, replay dan pemulihan.
- [REST awal](api/README.md) dan [strategi pengujian](engineering/testing.md) menyediakan antarmuka prioritas serta bukti yang diperlukan.
- [Review](architecture/review.md) mencatat koreksi dan batas validasi. DESIGN.md serta UI belum dibuat karena arah visual belum disetujui.

## Validasi dan batasan

Pemeriksaan dokumen meliputi tautan, whitespace, diagram source, JSON contoh, referensi requirement, label/atribusi dan metadata commit. Pengujian aplikasi, locking, rollback, isolasi DB, restore, printer dan deployment belum dijalankan karena belum ada implementasi. Schema/OpenAPI lengkap untuk fitur berikutnya harus dilengkapi sebelum implementasi.

O-001–05, presisi kuantitas, konfigurasi tarif/outlet, runtime, palet/tipografi dan lisensi tetap terbuka. Merge rancangan bukan bukti kesiapan produksi atau persetujuan otomatis pilihan bisnis.

## Langkah berikutnya

Kerjakan UX-PLAN-01 sampai UX-PLAN-04 pada backlog, dimulai dengan arah visual pengguna. Selesaikan keputusan produk dan versi dependency sebelum schema/scaffold terkait. Riwayat merge dan kecocokan remote diperiksa melalui Git.
