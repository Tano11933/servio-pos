# Dokumentasi Servio POS

Dokumentasi ditulis dalam Bahasa Indonesia. Identifier kode, nama endpoint, dan pesan commit menggunakan Bahasa Inggris. Dokumen berstatus rencana belum menjadi kontrak implementasi.

## Dokumen tersedia

| Dokumen | Fungsi | Status |
| --- | --- | --- |
| [Product brief](product/brief.md) | Sasaran, cakupan, dan batas pilot yang disepakati | Baseline |
| [PRD](product/prd.md) | Kebutuhan pilot dan kriteria penerimaan | Draft untuk review |
| [Matriks akses](product/access-matrix.md) | Usulan izin per peran/outlet | Menunggu review |
| [Matriks validasi](product/acceptance-matrix.md) | Pemetaan requirement ke skenario pengujian | Rancangan; belum dijalankan |
| [Aturan bisnis](product/business-rules.md) | Pesanan, dapur, meja, dan bahan | Usulan untuk review |
| [Aturan uang/shift/retur](product/settlement-rules.md) | Perhitungan, pembayaran, kas, refund, laporan | Usulan untuk review |
| [Contoh penerimaan](product/business-examples.md) | Expected result aturan | Rancangan; bukan pengujian aplikasi |
| [Arsitektur sistem](architecture/overview.md) | Container, modul, keamanan, deployment | Rancangan baseline |
| [Model data/ERD](architecture/data-model.md) | Entitas, tipe, scope, constraint, indeks | Belum menjadi migrasi |
| [ADR struktur](architecture/adr/001-modular-monolith.md) | Modular monolith dan trust boundary | Rancangan |
| [ADR transaksi](architecture/adr/002-transactions-and-recovery.md) | Locking, idempotency, retry, pemulihan | Belum diuji runtime |
| [Kontrak REST](api/README.md) | Operasi prioritas dan format pertukaran | Kontrak awal, bukan endpoint aktif |
| [Strategi pengujian](engineering/testing.md) | Lapisan bukti dan skenario kritis | Rancangan |
| [Review arsitektur](architecture/review.md) | Temuan, koreksi, dan batas validasi | Review dokumentasi |
| [Roadmap](roadmap.md) | Milestone dan checkpoint | Baseline |
| [Daftar keputusan](decisions.md) | Keputusan terkunci, default, dan hal terbuka | Tersedia |
| [Backlog](backlog.md) | Tugas berikutnya dengan kriteria selesai | Keputusan desain/UX dan scaffold |
| [Progres](progress.md) | Hasil, batasan, dan langkah berikutnya | Tersedia |
| [Panduan pengembangan](../AGENTS.md) | Aturan implementasi dan validasi | Tersedia |
| [Panduan kontribusi](../CONTRIBUTING.md) | Branch, commit, review, dan handoff | Tersedia |
| [Template PR](../.github/pull_request_template.md) | Format review perubahan | Tersedia |
| [Template tugas](../.github/ISSUE_TEMPLATE/roadmap-task.md) | Acceptance criteria tugas roadmap | Tersedia |
| [Template bug](../.github/ISSUE_TEMPLATE/bug-report.md) | Reproduksi dan bukti perbaikan | Tersedia |

## Dokumen yang dibuat sesuai jadwal

Daftar ini adalah rencana, bukan tautan ke file yang sudah tersedia. Hindari membuat file kosong seolah-olah spesifikasinya sudah selesai.

| Target | Dokumen / lokasi terencana | Isi minimum |
| --- | --- | --- |
| UX | `design/ux-spec.md` | Alur, wireframe, keyboard, responsivitas, empty/loading/error/offline states |
| Arah visual | `../DESIGN.md` pada root repository | Arah pengguna, persetujuan, dan alasan keputusan utama |
| Sebelum fitur terkait | Lengkapi JSON Schema/OpenAPI dari kontrak REST awal | Payload/response lengkap, batas field, izin, error, idempotency, konflik |
| Staging | Runbook: `operations/runbook.md` | Environment, migrasi, deployment, monitoring, insiden |
| Kesiapan pilot | Backup/restore: `operations/backup-restore.md` | RPO/RTO, retensi, akses, langkah restore, bukti latihan |
| Operasi outlet | Panduan operasional: `operations/pilot-guide.md` | Onboarding, kasir, dapur, owner, fallback operasional |
| Evaluasi kesiapan | `operations/pilot-readiness.md` | Checklist, bukti, masalah terbuka, keputusan layak pilot |

## Sumber kebenaran dan keterlacakan

- Product brief menetapkan batas produk; PRD menjabarkan kebutuhan serta penerimaannya.
- Aturan bisnis menetapkan perilaku; ERD/API menerjemahkannya menjadi kontrak teknis.
- ADR menyimpan alasan keputusan teknis, bukan menggantikan PRD.
- Issue/tugas merujuk requirement PRD; PR merujuk issue dan bukti pengujian.
- Perubahan kebutuhan memperbarui dokumen, kode, dan pengujian dalam perubahan terkait.
- Klaim selesai harus disertai bukti. Dokumen rancangan, hasil pengujian, dan persetujuan pengguna adalah status yang berbeda.
