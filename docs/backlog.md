# Backlog aktif

Milestone aktif: fondasi produk dan pengembangan. Roadmap menyimpan urutan besar; backlog merinci tugas yang siap dikerjakan. Issue merujuk tugas ini tanpa menduplikasi status yang saling bertentangan.

## Berikutnya: product requirements

Branch terencana: `docs/product-requirements`.

Prasyarat: baca product brief, roadmap, daftar keputusan, dan progres terbaru. Tidak memerlukan scaffold aplikasi.

| ID | Tugas | Kriteria selesai |
| --- | --- | --- |
| PRD-01 | Susun PRD dengan ID kebutuhan stabil | Fitur pilot memiliki aktor, perilaku, prioritas, dan acceptance criteria |
| PRD-02 | Susun matriks akses | Operasi owner/manager/kasir/dapur serta batas tenant/outlet tertulis; usulan dibedakan dari keputusan |
| PRD-03 | Susun skenario takeaway dan dine-in | Mencakup pesanan, tambahan, dapur, pembayaran, shift, pembatalan, retur, dan gangguan |
| PRD-04 | Hubungkan kebutuhan dengan pengujian | Kebutuhan kritis punya skenario positif, penolakan akses, serta duplikasi/konkurensi yang relevan |
| PRD-05 | Review batas pilot | Tanpa split bill/offline penuh/payment gateway; aturan belum final ditandai |

Commit dapat dipisahkan berdasarkan kebutuhan/cakupan, izin/alur penerimaan, dan penelusuran pengujian. Jumlah mengikuti perubahan nyata.

## Antrean berikutnya

1. Kunci state machine dan dampak stok/kas sebelum ERD/API final.
2. Buat arsitektur, ERD, dan ADR; susun kontrak sebelum fitur terkait.
3. Susun wireframe dan design brief; palet/tipografi belum disetujui.
4. Pilih runtime/dependency, scaffold aplikasi/database, siapkan CI, dan kaji printer.

Command Node/npm ditemukan tetapi belum ada versi Node aktif. Pilih runtime yang didukung dependency dan verifikasi versi sebelum scaffold.

## Kebutuhan eksternal

- Remote: `https://github.com/Tano11933/servio-pos.git`.
- Printer fisik dibutuhkan sebelum uji cetak; hosting dan akses outlet dibutuhkan untuk staging/pilot.
- Lisensi distribusi belum ditentukan.

Kebutuhan tersebut tidak menghalangi PRD. Jangan menyatakan pengujian perangkat atau layanan berhasil sebelum dilaksanakan.
