# Matriks akses pilot

Status: usulan untuk review pemilik, terkait ACC-02 dan ACC-03 pada [PRD](prd.md). Belum mengunci O-001. Hak diberikan berdasarkan membership tenant dan penugasan outlet; peran tidak otomatis memberikan akses global.

`Ya` berarti izin dalam outlet yang ditugaskan. `Sendiri` berarti hanya sumber daya milik pengguna tersebut. `Tidak` adalah penolakan default. Owner berada dalam tenant miliknya, bukan administrator lintas tenant.

| Operasi | Owner | Manager | Kasir | Dapur |
| --- | --- | --- | --- | --- |
| Kelola membership/peran/penugasan outlet | Ya | Tidak | Tidak | Tidak |
| Konfigurasi outlet, harga, pajak, service charge | Ya | Tidak | Tidak | Tidak |
| Kelola kategori/menu/varian/modifier/resep | Ya | Ya | Tidak | Tidak |
| Lihat katalog kasir | Ya | Ya | Ya | Tidak |
| Lihat item/catatan persiapan | Ya | Ya | Ya | Ya |
| Kelola bahan, penerimaan, penyesuaian, waste | Ya | Ya | Tidak | Tidak |
| Lihat laporan stok lengkap | Ya | Ya | Tidak | Tidak |
| Kelola master meja | Ya | Ya | Tidak | Tidak |
| Buat/ubah/kirim pesanan dan terima pembayaran | Ya | Ya | Ya | Tidak |
| Ubah status persiapan dapur | Ya | Ya | Tidak | Ya |
| Hapus item draft belum terkirim | Ya | Ya | Ya | Tidak |
| Batalkan item/pesanan terkirim | Ya | Ya | Tidak | Tidak |
| Proses retur/refund manual | Ya | Ya | Tidak | Tidak |
| Buka/tutup shift sendiri | Ya | Ya | Sendiri | Tidak |
| Catat kas masuk/keluar di shift sendiri | Ya | Ya | Sendiri | Tidak |
| Lihat ringkasan shift sendiri | Ya | Ya | Sendiri | Tidak |
| Lihat shift lain dan selisih outlet | Ya | Ya | Tidak | Tidak |
| Lihat/cetak ulang struk operasional outlet | Ya | Ya | Ya | Tidak |
| Lihat laporan bisnis, audit, dan ekspor | Ya | Ya | Tidak | Tidak |

## Aturan penerapan

- Backend menolak operasi tanpa izin walaupun klien menampilkan kontrol yang keliru.
- ID relasi, filter, file, ekspor, dan pekerjaan background harus mempertahankan tenant/outlet.
- Peran yang boleh membayar tetap memerlukan shift aktif sesuai aturan shift; izin saja tidak melewati prasyarat bisnis.
- Dapur menerima informasi yang diperlukan untuk persiapan, bukan nominal pembayaran, laporan usaha, atau pengelolaan pengguna.
- Tidak ada pengambilalihan shift atau persetujuan memakai akun orang lain. Jika dibutuhkan, ajukan alur tersendiri sebelum implementasi.
- Penolakan akses tidak mengubah data; perubahan izin dicatat. Pencabutan membership/peran berlaku pada request berikutnya.

## Poin review

Konfirmasi apakah manager boleh mengubah harga/konfigurasi, kasir boleh membatalkan item terkirim, dan kasir boleh melihat semua struk outlet. Usulan saat ini membatasi tindakan koreksi ke owner/manager. Sampai direview, jangan menganggap usulan sebagai otorisasi produk final.
