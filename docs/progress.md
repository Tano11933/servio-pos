# Progres proyek

## Status terbaru

| Item | Status |
| --- | --- |
| Produk | Servio POS |
| Milestone | Fondasi produk dan pengembangan |
| Hasil terbaru | Usulan aturan operasional, uang/shift/retur, dan contoh penerimaan |
| Review | Integrasi dokumen disetujui pemilik; pilihan yang ditandai usulan tetap perlu dikunci |
| Branch | `main`; aturan bisnis diintegrasikan dari `docs/business-rules` |
| Berikutnya | Review keputusan bisnis lalu arsitektur/model data |
| Repository | [Tano11933/servio-pos](https://github.com/Tano11933/servio-pos) |

## Catatan kerja tetap

Penamaan mengalir berdasarkan fitur dan milestone, tanpa label hari pada dokumen, branch, atau commit. Metadata commit memakai identitas Git pemilik repository tanpa atribusi tambahan; jangan membuat commit kosong untuk memenuhi jumlah.

## Hasil

- Dokumen kebutuhan sudah diintegrasikan pada main dengan persetujuan pemilik.
- Dokumen aturan bisnis diintegrasikan ke main atas permintaan pemilik; integrasi tidak otomatis menutup keputusan yang masih ditandai usulan.
- [Aturan operasional](product/business-rules.md): transisi pesanan/dapur/meja, snapshot, idempotency, konflik, reservasi, konsumsi dan waste.
- [Aturan uang/shift/retur](product/settlement-rules.md): total eksak, alokasi refund, pelunasan, kas shift, koreksi historis dan laporan.
- [Contoh penerimaan](product/business-examples.md): 22 expected result untuk hitungan, retry, konkurensi, refund dan penutupan.
- PRD, matriks validasi, indeks, roadmap serta keputusan diselaraskan tanpa menganggap pilihan operasional sudah disetujui.

## Validasi dokumen

Periksa whitespace dan tautan lokal, ID aturan/contoh serta referensinya, hitungan contoh, metadata commit dan penamaan. Pemeriksaan aritmetika contoh tidak membuktikan aplikasi, locking atau rollback sudah berjalan. Pengujian aplikasi dalam matriks validasi masih belum dijalankan.

## Batasan dan review

Belum ada source, database, dependency, build/test aplikasi, deployment, uji printer atau restore. Palet/tipografi, lisensi, runtime dan konfigurasi outlet belum dikunci. Review utama: IDR/pembulatan/basis pajak, konsumsi saat persiapan, kapan meja lepas, refund saat persiapan, izin diskon, atribusi kas shift, dan masa retur. Semuanya tetap usulan.

## Langkah berikutnya

Review O-001 sampai O-005, catat keputusan, lalu kerjakan ARCH-01 sampai ARCH-04 pada backlog. Branch baru mengikuti dependensi yang sudah direview. Status push dan hash akhir diperiksa melalui Git, bukan disimpulkan dari URL repository.
