# Aturan bisnis Servio POS

Status: usulan baseline untuk review, bukan aturan yang sudah disetujui atau diimplementasikan. Melengkapi [PRD](prd.md), [matriks akses](access-matrix.md), dan [aturan uang/shift/retur](settlement-rules.md). Semua aturan BR di bawah menunggu persetujuan pemilik. Batas scope tetap mengikuti product brief.

## BR-01: otorisasi dan perintah

Setiap perintah memverifikasi sesi, membership tenant, outlet, izin, dan versi sumber daya di server. Semua relasi harus berada dalam tenant/outlet yang sah. Pemeriksaan dan mutasi kritis harus atomik; penolakan tidak menyisakan perubahan parsial.

Mutasi pesanan, pengiriman batch, perubahan status dapur, pembayaran, pembatalan, retur, dan mutasi shift memakai identitas operasi stabil. Identitas sama/payload sama mengembalikan hasil semula; payload berbeda ditolak. Identitas operasi berbeda tetap tunduk pada constraint bisnis agar tidak menggandakan batch/pelunasan. Pelaku kehilangan izin tidak boleh mengakses hasil lama melalui replay.

Setiap mutasi memakai versi yang diharapkan; konflik memerlukan pemuatan ulang sebelum tindakan baru. Waktu server dan zona waktu outlet menjadi acuan pencatatan/laporan; urutan tidak ditentukan jam perangkat.

## BR-02: pesanan dan item

Usulan status pesanan: `DRAFT`, `OPEN`, `CLOSED`, `CANCELLED`. Status pembayaran, persiapan item, serta penggunaan meja disimpan terpisah.

| Transisi | Syarat | Efek |
| --- | --- | --- |
| Baru → DRAFT | Peran kasir berizin, outlet aktif | Belum ada reservasi atau antrean dapur |
| DRAFT → OPEN | Batch pertama valid dan bahan tersedia | Kunci snapshot item/harga/resep; reservasi bahan; buat tiket dapur atomik |
| OPEN → OPEN | Batch tambahan; belum dibayar | Hanya item baru dikirim; item lama tidak dikirim ulang |
| OPEN → CLOSED | Pembayaran lunas dan seluruh item terkirim berstatus READY atau CANCELLED | Lepas meja dan hentikan perubahan tagihan |
| DRAFT → CANCELLED | Penghapusan draft oleh pembuat/peran berizin | Tanpa efek stok atau refund |
| OPEN → CANCELLED | Belum dibayar, seluruh item dibatalkan oleh peran berizin | Lepas meja, akhiri tagihan, pertahankan riwayat |

Pembayaran dapat mendahului kesiapan dapur. Status PAID dengan persiapan belum selesai tetap OPEN tetapi tagihan terkunci. Saat item terakhir siap, penutupan dijalankan atomik. Pembayaran yang datang setelah semua item siap dapat langsung menutup pesanan. Sesudah refund, penutupan memakai bukti pembayaran historis dan finalitas seluruh item, bukan mensyaratkan status pembayaran tetap PAID. Tagihan pernah dibayar yang seluruh itemnya akhirnya siap/dibatalkan menjadi CLOSED, bukan CANCELLED; refund tidak membuka ulang CLOSED.

Item draft dapat diubah/dihapus. Item terkirim tidak diedit harga/resep/kuantitasnya secara langsung: sebelum pembayaran, peran berizin membatalkan item lalu menambahkan item pengganti. Pilot membatalkan seluruh satu baris item; pembatalan sebagian kuantitas baris tidak termasuk baseline. Baris tambahan memiliki identitas baru.

Pesanan harus memiliki sedikitnya satu item aktif dan total positif untuk menerima pembayaran. Draft yang menjadi kedaluwarsa akibat perubahan harga/resep/ketersediaan harus ditampilkan ulang untuk konfirmasi sebelum dikirim; server tidak mengganti harga diam-diam. Snapshot terkirim tidak berubah oleh pembaruan katalog. Tidak ada pembukaan kembali tagihan setelah lunas; koreksi menggunakan retur/refund.

## BR-03: dapur

Status per baris item terkirim: `QUEUED`, `PREPARING`, `READY`, `CANCELLED`. Batch adalah kelompok pengiriman, bukan salinan tagihan.

| Transisi | Pelaku | Dampak bahan |
| --- | --- | --- |
| QUEUED → PREPARING | Dapur/manager/owner berizin | Konsumsi bahan dari reservasi sekali |
| PREPARING → READY | Dapur/manager/owner berizin | Tidak mengonsumsi ulang |
| QUEUED → CANCELLED | Manager/owner berizin; belum dibayar | Lepas reservasi |
| PREPARING/READY → CANCELLED | Manager/owner berizin; belum dibayar; alasan wajib | Bahan tetap terpakai; klasifikasikan sebagai waste terkait |

Tidak ada transisi mundur atau lompatan QUEUED → READY; menu tanpa proses masak tetap melewati pencatatan mulai/siap. Pembatalan dibayar ditangani melalui BR-06, bukan jalur pembatalan belum bayar. Replay transisi yang sudah sukses mengembalikan hasil sebelumnya; transisi dari status yang tidak sesuai ditolak.

Dapur melihat item dibatalkan, batch tambahan, catatan, dan identitas pesanan/meja. Reconnect mengambil snapshot server terbaru; event hilang/berulang tidak mengubah kebenaran transaksi.

## BR-04: bahan, reservasi, dan waste

`tersedia = on_hand − reserved`. Kuantitas menggunakan desimal eksak dalam satuan dasar. Resep menyimpan versi dan kebutuhan per porsi/modifier; konversi hanya dalam dimensi yang sama. Pengiriman ditolak jika resep tidak lengkap atau kebutuhan tidak dapat direpresentasikan dalam presisi satuan yang dipilih. Presisi kuantitas dikunci sebelum schema dibuat.

- Saat kirim: reservasi kebutuhan snapshot untuk semua item baru secara atomik; kekurangan satu bahan menggagalkan seluruh batch.
- Saat mulai persiapan: kurangi `on_hand` dan `reserved` sebesar kebutuhan yang sama, tambahkan ledger konsumsi sekali. Tidak boleh negatif.
- Saat batal sebelum persiapan: kurangi `reserved`, tanpa menaikkan `on_hand` karena belum ada konsumsi.
- Saat batal sesudah persiapan: jangan mengurangi `on_hand` lagi. Hubungkan waste ke konsumsi yang sudah ada; laporan membedakan konsumsi produksi dan porsi yang dibuang tanpa menggandakan kuantitas ledger.
- Waste mandiri (bahan rusak) mengurangi on_hand tersedia sekali. Penyesuaian negatif dan waste mandiri tidak boleh menjatuhkan on_hand di bawah reserved.
- Stok awal, penerimaan, penyesuaian, dan waste memiliki sumber, alasan, pelaku, dan waktu. Koreksi ledger menggunakan entri pembalik yang diaudit, bukan penghapusan riwayat.

Contoh: susu on_hand 1.000 ml, reserved 0; kirim kebutuhan 200 ml → on_hand 1.000, reserved 200, tersedia 800. Mulai persiapan → on_hand 800, reserved 0, tersedia 800. Batal sesudah mulai → on_hand tetap 800; 200 ml dicatat sebagai konsumsi terkait waste, bukan dikurangi lagi.

Pengiriman, persiapan, stok koreksi, dan pembatalan yang bersamaan harus berserialisasi terhadap saldo/item terkait. Mengulang pembayaran tidak mengubah stok karena konsumsi terjadi pada persiapan, bukan pembayaran.

## BR-05: meja dan penutupan

Meja memiliki paling banyak satu pesanan aktif (DRAFT atau OPEN) per outlet. Membuka draft dine-in mencadangkan meja secara atomik; menghapus/membatalkan draft melepaskannya. Status tampilan tersedia/terpakai diturunkan dari hubungan pesanan aktif, bukan tombol status bebas.

Usulan sederhana pilot: meja tetap terpakai sampai pembayaran dan persiapan selesai, atau pesanan dibatalkan seluruhnya. Refund tidak membuka kembali meja/pesanan. Reservasi pelanggan, pergantian meja, penggabungan tagihan, dan status bersih-bersih berada di luar baseline.

## BR-06: pembatalan setelah pembayaran

Perintah CAN-01 hanya untuk tagihan belum dibayar. Pada tagihan PAID gunakan retur/refund sesuai [BR-09](settlement-rules.md), dengan alasan dan izin manager/owner. Untuk item belum mulai, refund harus sekaligus membatalkan kuantitas yang dikembalikan dan melepas reservasinya secara atomik. Pada item disiapkan, bahan tidak otomatis kembali; kuantitas yang dibuang diberi disposisi/waste tanpa konsumsi kedua.

Pilot menunda refund atas item PREPARING sampai READY, agar tidak ada penghentian sebagian proses dapur yang ambigu. Refund sebagian kuantitas pada QUEUED mempertahankan sisa kuantitas aktif; mulai persiapan harus membaca kuantitas/saldo reservasi terkini. Perlombaan refund dan mulai persiapan hanya memenangkan satu urutan transaksi yang sah.

## Review yang diperlukan

- Konfirmasi konsumsi bahan saat PREPARING, pembatalan seluruh baris sebelum bayar, dan penguncian tagihan setelah bayar.
- Konfirmasi kapan meja dilepas dan penundaan refund item PREPARING.
- Konfirmasi izin pada matriks akses; penyusunan dokumen tidak menggantikan keputusan pemilik.

BR-01 sampai BR-06 memetakan ACC-01/02/03, INV-01/02/03, ORD-01/02/03/04, KIT-01, PAY-03/04, CAN-01, RET-01, dan AUD-01. Skenario serta expected result ada pada [contoh penerimaan](business-examples.md).
