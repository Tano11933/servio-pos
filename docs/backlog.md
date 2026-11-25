# Backlog aktif

Milestone: fondasi produk dan pengembangan. Penamaan berdasarkan fitur/milestone tanpa label hari pada dokumen, branch, atau commit. Identitas commit memakai pemilik repository tanpa atribusi tambahan.

## Hasil terkini

PRD dan aturan bisnis tersedia di main. Rancangan arsitektur disusun melalui `docs/system-architecture`, direview, dan diotorisasi untuk integrasi setelah validasi. Dokumen ini tidak mengklaim fitur telah diimplementasikan.

| Tugas | Hasil | Status |
| --- | --- | --- |
| ARCH-01 | Diagram container, batas modul, sesi, tenant, dapur, printer dan deployment | Rancangan tersedia |
| ARCH-02 | ERD, kamus data, snapshot, scope FK, uang/kuantitas, constraint, audit | Rancangan; belum migrasi |
| ARCH-03 | ADR struktur dan transaksi: locking, idempotency, retry, pemulihan | Rancangan; belum pengujian runtime |
| ARCH-04 | REST prioritas, format error/uang dan strategi pengujian | Kontrak awal; schema lengkap sebelum fitur terkait |

## Keputusan sebelum implementasi

O-001 sampai O-005 tetap usulan: izin, nilai uang/basis pajak, konsumsi bahan, status/meja dan retur. Arsitektur mempertahankan dependensi tersebut. Presisi bahan/rate, konfigurasi outlet dan versi runtime wajib dikunci sebelum schema/scaffold terkait. Jangan menyebut merge dokumen sebagai persetujuan kebijakan outlet.

## Berikutnya: desain dan UX

Branch terencana `docs/product-design`, dari main yang memuat arsitektur terintegrasi.

| ID | Tugas | Kriteria selesai |
| --- | --- | --- |
| UX-PLAN-01 | Design brief bersama pemilik | Karakter produk, warna, tipografi, prioritas perangkat dan alasan visual disetujui; simpan di DESIGN.md root |
| UX-PLAN-02 | Alur dan wireframe | Kasir takeaway/dine-in, dapur, pembayaran, koreksi, shift dan konflik versi tersambung |
| UX-PLAN-03 | State dan aksesibilitas | Keyboard, fokus, sentuhan, loading/empty/error/reconnect, draft dan pembaruan PWA terdokumentasi |
| UX-PLAN-04 | Review keterlacakan | Kontrol UI mengikuti izin serta kontrak, tanpa memberi kesan pembayaran otomatis atau offline penuh |

Sesudahnya: pilih runtime/dependency/lockfile, lengkapi schema kontrak prioritas, scaffold frontend/backend/database, CI dan kajian printer. Node sebelumnya belum memiliki versi aktif; verifikasi ulang sebelum scaffold.

## Kebutuhan eksternal

Remote: `https://github.com/Tano11933/servio-pos.git`. Printer fisik, hosting, akses outlet, lisensi distribusi dan target RPO/RTO/beban masih dibutuhkan pada tahap terkait.
