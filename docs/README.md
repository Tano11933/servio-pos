# Dokumentasi Servio POS

Dokumentasi ditulis dalam Bahasa Indonesia. Identifier kode, nama endpoint, dan pesan commit menggunakan Bahasa Inggris. Dokumen berstatus rencana belum menjadi kontrak implementasi.

## Dokumen tersedia

| Dokumen | Fungsi | Status |
| --- | --- | --- |
| [Product brief](product/brief.md) | Sasaran, cakupan, dan batas pilot yang disepakati | Baseline |
| [Roadmap](roadmap.md) | Milestone dan checkpoint | Baseline |
| [Daftar keputusan](decisions.md) | Keputusan terkunci, default, dan hal terbuka | Tersedia |
| [Backlog](backlog.md) | Tugas berikutnya dengan kriteria selesai | PRD disiapkan |
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
| Kebutuhan | PRD: `product/prd.md` | Persona/peran, requirement ID, prioritas, acceptance criteria, batas fitur |
| Aturan bisnis | `product/business-rules.md` | Transisi status, harga, pembulatan, dapur, konsumsi bahan, pembatalan, retur, shift |
| Arsitektur | `architecture/overview.md` | Diagram konteks/container, modul, batas transaksi, tenant, sesi, deployment |
| Model data | `architecture/data-model.md` | ERD, kamus data awal, FK, unique constraint, nilai uang, kuantitas, waktu, snapshot |
| Keputusan teknis | ADR: `architecture/adr/` | Konteks, keputusan, konsekuensi, status, dan alternatif |
| UX | `design/ux-spec.md` | Alur, wireframe, keyboard, responsivitas, empty/loading/error/offline states |
| Arah visual | `design/brief.md` | Arah pengguna, persetujuan, dan alasan keputusan utama |
| Sebelum fitur terkait | API: `api/README.md` dan OpenAPI saat backend tersedia | Endpoint, payload, izin, error, idempotency, konflik, pagination |
| Scaffold dan fitur | Strategi pengujian: `engineering/testing.md` | Pemetaan requirement ke pengujian dan cara menjalankannya |
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
