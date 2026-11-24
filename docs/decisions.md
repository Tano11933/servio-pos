# Daftar keputusan

Status dibedakan antara keputusan pengguna yang sudah disepakati, default kerja yang dapat direvisi melalui review, dan keputusan terbuka. Pemilihan teknologi tidak membuktikan fitur sudah ada.

## Disepakati

| ID | Keputusan | Dasar / konsekuensi |
| --- | --- | --- |
| D-001 | F&B, takeaway dan layanan meja | Kedua alur masuk pilot |
| D-002 | React/Vite/TypeScript, NestJS, PostgreSQL, Prisma | Modular monolith; SQL eksplisit diizinkan untuk operasi kritis |
| D-003 | Online dahulu, web responsif dan PWA Android | Gangguan dipulihkan; transaksi baru memerlukan server |
| D-004 | Bahan dan resep | Perlu konversi, snapshot resep, ledger, dan waste |
| D-005 | Layar dapur sederhana | Struk pelanggan menjadi fokus thermal awal |
| D-006 | Pembayaran manual | QRIS/EDC dikonfirmasi kasir; tanpa callback penyedia |
| D-007 | Pilot satu outlet, satu mata uang, pembayaran penuh | Split bill dan pembayaran sebagian ditunda |
| D-008 | Target dua bulan, review pemilik 1–2 jam/hari | Milestone dan readiness menentukan kemajuan serta kelayakan pilot |
| D-009 | Branch per perubahan/fitur kecil dan 3–4 commit bermakna/hari | Merge commit mempertahankan riwayat; tidak membuat aktivitas semu |
| D-010 | Nama Servio POS, repository public `Tano11933/servio-pos` | Remote: `https://github.com/Tano11933/servio-pos.git` |
| D-011 | Penamaan berdasarkan fitur dan milestone | Tidak memakai label hari pada dokumen, branch, atau commit; identitas commit memakai pemilik repository tanpa atribusi tambahan |

Catatan kerja: gunakan penamaan yang mengalir berdasarkan fitur/milestone pada dokumen, branch, dan commit. Metadata commit tetap memakai identitas pemilik tanpa atribusi tambahan. Aturan ini berlaku untuk sesi lanjutan.

## Default kerja

| ID | Default | Alasan |
| --- | --- | --- |
| W-001 | Dokumen Bahasa Indonesia, identifier dan commit Bahasa Inggris | Memudahkan review pemilik dan konsistensi kode |
| W-002 | Kemajuan mengikuti hasil aktual dan checkpoint | Tanggal dan jumlah commit tidak membuktikan fitur selesai |
| W-003 | Dependency dipasang saat scaffold | Pilih runtime yang didukung dan dapat diuji |
| W-004 | Branch dokumentasi diserahkan untuk review sebelum merge | Mengikuti workflow PR dan persetujuan hasil |
| W-005 | Lisensi distribusi belum ditetapkan | Perlu keputusan pemilik; public tidak otomatis berarti berlisensi open source |

## Keputusan terbuka

Usulan rinci tersedia pada [aturan bisnis](product/business-rules.md) (BR-01–06), [uang/shift/retur](product/settlement-rules.md) (BR-07–10), dan [contoh penerimaan](product/business-examples.md). O-002 sampai O-005 tetap terbuka sampai direview; dokumentasi bukan persetujuan otomatis.

Poin utama: usulan IDR dan half-up satu rupiah; konsumsi bahan saat PREPARING; meja dilepas setelah pelunasan historis dan item final; refund item PREPARING ditunda; diskon hanya owner/manager; refund tercatat pada shift pelaku yang aktif. Tarif dan data outlet belum ditetapkan.

| ID | Keputusan | Batas waktu | Cara menyelesaikan |
| --- | --- | --- | --- |
| O-001 | Izin per peran dan outlet | Implementasi akses | [Matriks usulan](product/access-matrix.md) tersedia; menunggu review pemilik |
| O-002 | Mata uang, pembulatan, diskon, pajak, service charge | Model uang dan pembayaran | Representasi eksak, contoh hitungan, dan konfigurasi terverifikasi |
| O-003 | Reservasi, konsumsi, pelepasan bahan | Model pesanan/inventori | Transisi termasuk persiapan dan pembatalan |
| O-004 | Status pesanan, pembayaran, dan dapur | ERD/API terkait | State transition serta aturan perubahan bersamaan |
| O-005 | Retur makanan/bahan | Model retur | Bedakan refund dan disposisi; jangan otomatis membalik resep |
| O-006 | ERD dan constraint tenant/outlet | Migrasi awal | Relasi, constraint, dan query tidak melintasi tenant |
| O-007 | Palet, tipografi, dan arah visual | UI final | Design brief pemilik |
| O-008 | Runtime, package manager, dependency | Scaffold | Dukungan resmi, versi, lockfile, dan ADR |
| O-009 | Model thermal, koneksi, jalur cetak | Pembelian dan integrasi perangkat | Kajian awal pada fondasi; uji fisik sebelum kesiapan pilot |
| O-010 | Lisensi distribusi | Pemberian hak distribusi | Keputusan pemilik |
| O-011 | Hosting, domain, secret, anggaran | Staging | Sesuaikan anggaran dan akses pemilik |
| O-012 | RPO, RTO, retensi backup | Uji restore | Sepakati target lalu ukur lewat latihan |
| O-013 | Beban dan respons target | Uji performa pilot | Perangkat, jumlah kasir, volume data, dan target terukur |

Keputusan teknis yang sudah dirinci dipindahkan/dirujuk ke ADR tanpa menghapus jejak perubahannya. Jika keputusan memengaruhi cakupan atau jadwal, perbarui roadmap dan PRD dalam perubahan yang sama.
