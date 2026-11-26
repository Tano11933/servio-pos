# Product requirements Servio POS

Status: draft untuk review pemilik. Cakupan mengikuti [product brief](brief.md); belum ada implementasi fitur. Matriks izin dan detail perilaku yang disebut usulan belum menjadi keputusan final.

## Tujuan dan batas

Pilot satu outlet dengan satu mata uang dan pembayaran penuh per tagihan. Pengguna: owner, manager, kasir, dan dapur. Fokusnya mencatat operasi F&B dari pesanan ke dapur, bahan, pembayaran, dan rekonsiliasi kas.

Tidak termasuk split bill, pembayaran sebagian, gabung/pindah meja, offline penuh, payment gateway, transfer antaroutlet, costing/HPP lengkap, pembelian pemasok lengkap, ERP/MRP, onboarding mandiri, atau billing SaaS.

Kebutuhan berikut berprioritas wajib untuk pilot. Implementasi mengikuti dependensi roadmap; tabel ini tidak menetapkan bentuk endpoint atau schema database final.

## Kebutuhan dan kriteria penerimaan

| ID | Kemampuan / aktor | Kriteria penerimaan |
| --- | --- | --- |
| ACC-01 | Login dan sesi / semua peran | Pengguna aktif dapat masuk dan keluar; kredensial salah ditolak; sesi kedaluwarsa/dicabut tidak dapat menjalankan operasi bisnis |
| ACC-02 | Batas tenant/outlet / semua peran | Setiap operasi memverifikasi membership dan izin server; manipulasi ID atau outlet tidak membuka data usaha lain, termasuk ekspor dan relasi |
| ACC-03 | Pengelolaan akses / owner | Hanya pemegang izin yang dapat mengubah membership/peran; pencabutan akses efektif pada request berikutnya dan tercatat di audit |
| CAT-01 | Katalog / owner, manager | Kategori, menu, varian, modifier, harga, dan ketersediaan dapat dikelola; input tidak valid ditolak; menu nonaktif tidak dapat ditambahkan sebagai item baru |
| CAT-02 | Pemilihan menu / kasir | Pencarian dan pilihan modifier menghasilkan item yang jelas; backend menolak harga atau pilihan tidak sah dari klien |
| INV-01 | Bahan dan satuan / owner, manager | Bahan memiliki satuan dasar; konversi positif dalam dimensi sama valid; konversi berat ke volume tanpa aturan sah ditolak |
| INV-02 | Resep / owner, manager | Menu/varian/modifier memiliki kebutuhan bahan sesuai aturan; perubahan resep tidak mengubah kebutuhan historis pesanan yang sudah dikunci |
| INV-03 | Pergerakan stok / owner, manager | Stok awal, penerimaan, penyesuaian, dan waste memiliki sumber, alasan, pelaku, waktu, serta kuantitas; saldo dapat direkonsiliasi dengan ledger |
| ORD-01 | Takeaway / kasir | Kasir membuat draft, memilih item/modifier/catatan, mengubah kuantitas, dan mengirim pesanan; total final berasal dari server |
| ORD-02 | Dine-in / kasir | Pesanan terikat meja dan tagihan terbuka; item tambahan dikirim sebagai batch baru tanpa mengirim ulang item lama; pemakaian meja bersamaan tidak menghasilkan tagihan aktif ganda yang tidak sah |
| ORD-03 | Konflik perubahan / kasir | Perubahan terhadap versi pesanan yang sudah tertinggal ditolak sebagai konflik; tampilan memuat ulang data tanpa menimpa perubahan lain secara diam-diam |
| ORD-04 | Dampak bahan / kasir, dapur | Pengiriman/persiapan/pembatalan mengubah reservasi atau konsumsi sesuai aturan bisnis yang dikunci; perebutan bahan tidak melampaui stok yang diizinkan |
| KIT-01 | Antrean / dapur | Hanya item yang dikirim tampil; batch tambahan dibedakan; transisi menunggu/diproses/siap mengikuti izin dan aturan; reconnect memuat kondisi terbaru |
| PAY-01 | Perhitungan / kasir | Server menghitung harga, diskon, pajak, service charge, pembulatan, dan total secara eksak; snapshot transaksi tidak berubah saat katalog diperbarui |
| PAY-02 | Pembayaran manual / kasir | Tunai menyimpan jumlah diterima dan kembalian, menolak kekurangan; QRIS/EDC memerlukan konfirmasi kasir dan tidak diklaim terverifikasi penyedia; nilai pembayaran menyelesaikan satu tagihan penuh |
| PAY-03 | Deduplikasi / kasir | Pengulangan operasi dengan identitas sama dan payload sama mengembalikan hasil yang sama; payload berbeda ditolak; pembayaran bersamaan tidak melunasi tagihan dua kali |
| PAY-04 | Pemulihan hasil / kasir | Setelah timeout, kasir dapat mengecek hasil dengan identitas operasi yang sama; status tak diketahui tidak dianggap otomatis gagal atau dicoba ulang sebagai transaksi baru |
| REC-01 | Struk dan riwayat / kasir, manager, owner | Struk berasal dari transaksi tersimpan; pencarian dibatasi outlet/izin; gagal cetak dan cetak ulang tidak membuat penjualan baru |
| SHF-01 | Buka shift / kasir | Saldo awal dan pelaku tercatat; aturan shift aktif mencegah penggunaan shift tidak sah; penerimaan pembayaran memiliki hubungan shift yang dapat ditelusuri |
| SHF-02 | Mutasi dan tutup shift / kasir, manager | Kas masuk/keluar memiliki alasan; sistem menampilkan kas yang diharapkan, aktual, serta selisih; shift tertutup tidak menerima mutasi baru; pembayaran bersamaan dengan penutupan ditangani konsisten |
| CAN-01 | Pembatalan / peran berizin | Alasan wajib; pembatalan mempertahankan riwayat dan mematuhi status persiapan/pembayaran; dapur serta stok berubah sesuai aturan tanpa efek berulang |
| RET-01 | Retur dan refund manual / peran berizin | Mengacu transaksi/item asal; akumulasi retur tidak melampaui kuantitas/nilai yang dapat diretur; refund dan dampak kas terlacak; bahan resep tidak otomatis kembali ke stok |
| AUD-01 | Audit / owner, manager | Perubahan akses, stok, pembatalan, retur, dan tindakan sensitif mencatat pelaku, waktu, sumber serta alasan; akses ke audit dibatasi dan tidak memuat secret |
| REP-01 | Laporan / owner, manager | Penjualan, pembayaran, menu, diskon, pembatalan, retur, dan shift dapat ditelusuri ke transaksi sumber; rentang waktu memakai zona waktu outlet |
| REP-02 | Stok dan ekspor / owner, manager | Saldo, pemakaian bahan, waste, stok minimum, dan CSV konsisten dengan ledger; filter/export tidak dapat melampaui izin tenant/outlet |
| UX-01 | Browser dan Android / pengguna berizin | Alur kasir dapat digunakan dengan sentuhan dan keyboard; fokus terlihat; loading, kosong, validasi, error, dan koneksi putus memiliki respons yang dapat dipahami |
| RES-01 | PWA dan draft / kasir | PWA dapat dipasang pada perangkat sasaran; draft dipisahkan per pengguna/outlet; logout/pergantian akun tidak membocorkan draft; pengesahan transaksi tetap memerlukan server |
| RES-02 | Pembaruan aplikasi / pengguna | Pembaruan aset tidak menghapus draft atau mengirim pembayaran ulang; reconnect memeriksa hasil operasi tertunda |
| PRT-01 | Thermal / kasir | Satu model dan koneksi didokumentasikan setelah uji fisik; struk terbaca, kegagalan dapat ditangani, cetak ulang aman; tidak menjanjikan dukungan universal |
| OPS-01 | Deployment dan pemantauan / pengelola | Staging dapat dibangun/deploy dengan konfigurasi terdokumentasi; health, log, dan error membantu menelusuri kegagalan tanpa membocorkan secret/data sensitif |
| OPS-02 | Backup dan pemulihan / pengelola | RPO/RTO dan retensi disepakati; backup direstore ke lingkungan terpisah; hasil dan waktu pemulihan diverifikasi sebelum pilot |
| OPS-03 | Beban pilot / pengelola | Target perangkat, jumlah kasir, volume data, dan respons ditetapkan sebelum pengukuran; hasil aktual dicatat dan penghambat diselesaikan |

## Alur penerimaan

1. **Takeaway:** kasir berizin membuka shift, membuat draft, memilih menu/modifier, mengirim batch, lalu menerima pembayaran penuh. Dapur melihat pesanan dan memperbarui persiapan. Struk, bahan, dan pembayaran dapat ditelusuri tanpa mensyaratkan pembayaran selalu sesudah persiapan selesai.
2. **Dine-in:** kasir membuka tagihan meja, mengirim item, menambah batch, dan melunasi seluruh tagihan. Perubahan bersamaan tidak menghilangkan item; meja kembali tersedia sesuai aturan penutupan yang masih perlu dikunci.
3. **Gangguan checkout:** server berhasil memproses tetapi respons hilang. Klien memeriksa identitas operasi yang sama; hasil hanya satu pembayaran, satu efek stok yang sesuai, dan satu kontribusi ke laporan.
4. **Pembatalan:** uji sebelum dan setelah persiapan. Otorisasi/alasan wajib; item dibatalkan terlihat di dapur; pelepasan reservasi dibedakan dari waste bahan terpakai.
5. **Retur:** transaksi tersimpan menjadi sumber kuantitas dan nilai; dua refund bersamaan tidak melampaui batas; kas dan laporan tetap dapat direkonsiliasi.
6. **Penutupan shift:** saldo awal + penerimaan tunai + kas masuk − refund tunai terkait − kas keluar menjelaskan kas yang diharapkan. QRIS/EDC tidak menambah kas fisik; atribusi refund ke shift ditentukan pada aturan bisnis.
7. **Isolasi:** akun usaha lain mencoba ID pesanan, bahan, meja, struk, dan ekspor. Semua jalur menolak akses; tidak ada perubahan data akibat request ditolak.

## Batas keputusan

[Matriks akses](access-matrix.md) adalah usulan review. Waktu konsumsi bahan, status/transisi, aturan shift, pembulatan, konfigurasi pajak, dan kebijakan retur tetap mengikuti keputusan terbuka pada [daftar keputusan](../decisions.md). Tabel di atas menetapkan kemampuan dan invariannya, bukan memilih detail tersebut diam-diam.

Usulan perilaku rinci ada pada [aturan operasional](business-rules.md) dan [aturan uang/shift/retur](settlement-rules.md), dengan [contoh penerimaan](business-examples.md). Dokumen tersebut belum mengunci keputusan pemilik; termasuk batas pembatalan baris, hak diskon, dan refund saat persiapan yang perlu direview sebelum implementasi.

Nama produk adalah Servio POS. Arah visual pemilik dan token turunan tersedia pada [DESIGN.md](../../DESIGN.md); [UX/wireframe](../design/ux-spec.md) serta [state interaksi](../design/interaction-states.md) merinci perilaku. Belum ada UI berjalan; detail layout dan tipografi aktual tetap perlu validasi implementasi.

## Syarat penerimaan pilot

Seluruh kebutuhan wajib memiliki bukti sesuai [matriks validasi](acceptance-matrix.md). Tidak ada masalah kritis salah hitung, kehilangan data, pelanggaran akses, atau duplikasi transaksi. Uji printer fisik, restore database, dan simulasi outlet harus benar-benar dilaksanakan. Persetujuan dokumen tidak berarti aplikasi sudah lolos penerimaan.
