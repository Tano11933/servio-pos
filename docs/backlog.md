# Backlog aktif

Milestone: fondasi produk dan pengembangan. Gunakan fitur/milestone tanpa label hari pada dokumen, branch, atau commit; identitas Git pemilik tanpa atribusi tambahan.

## Hasil terkini

PRD-01 sampai PRD-05 tersusun dan sudah diintegrasikan ke main. Branch `docs/business-rules` memuat usulan berikut, belum disetujui atau diimplementasikan:

| Tugas | Hasil | Status |
| --- | --- | --- |
| BIZ-01 | Status pesanan, pembayaran, dapur, meja, prasyarat dan konflik | Usulan BR-01/02/03/05 |
| BIZ-02 | Snapshot, reservasi, konsumsi, pembatalan dan waste | Usulan BR-04/06 |
| BIZ-03 | Uang eksak, pembulatan, diskon, service, tax dan alokasi refund | Usulan BR-07; tarif outlet terbuka |
| BIZ-04 | Shift, retur, batas kumulatif dan refund lintas shift | Usulan BR-08/09/10 |
| BIZ-05 | Keterlacakan keputusan, PRD, izin dan validasi | Dokumen diselaraskan; keputusan pemilik menunggu review |

## Berikutnya: review keputusan dan arsitektur

1. Review O-001 sampai O-005: izin, mata uang/basis pajak, kapan konsumsi, kapan lepas meja, refund saat persiapan, atribusi shift dan masa retur.
2. Catat persetujuan/perubahan eksplisit, perbarui aturan serta contoh yang terdampak. Jangan menandai usulan sebagai keputusan final tanpa review.
3. Mulai `docs/system-architecture` setelah dependensi aturan diintegrasikan; bila memakai branch belum merge, catat dependensinya.

| ID | Pekerjaan | Kriteria selesai |
| --- | --- | --- |
| ARCH-01 | Diagram konteks/container dan batas modul | Alur frontend–API–database, otorisasi, dapur, cetak dan operasi tergambar |
| ARCH-02 | ERD dan kamus data | Tenant/outlet, snapshot, versi, ledger, uang/kuantitas, constraint dan audit konsisten dengan aturan yang dikunci |
| ARCH-03 | ADR transaksi, idempotency dan konkurensi | Batas atomik, serialisasi saldo/pesanan/shift, replay dan pemulihan ditetapkan |
| ARCH-04 | Kontrak awal API dan strategi validasi | Operasi prioritas punya izin, input/output, konflik dan skenario database nyata |

Sesudahnya: `DESIGN.md` di root, UX/wireframe, runtime/dependency, scaffold, CI, dan kajian printer sesuai roadmap. Belum ada arah visual disetujui. Node sebelumnya belum memiliki versi aktif; verifikasi sebelum scaffold.

## Kebutuhan eksternal

Remote: `https://github.com/Tano11933/servio-pos.git`. Printer fisik, hosting, akses outlet, serta lisensi distribusi masih perlu disiapkan pada tahap terkait.
