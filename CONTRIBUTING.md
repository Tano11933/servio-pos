# Kontribusi dan review

## Ritme proyek

Roadmap mengikuti milestone dengan target pilot dalam dua bulan. Setiap sesi menghasilkan perubahan yang dapat diperiksa. Lakukan review dan penyesuaian backlog secara berkala; hindari penomoran hari dalam dokumen, branch, dan commit.

Saat mulai sesi, baca [progres](docs/progress.md), [backlog](docs/backlog.md), serta [roadmap](docs/roadmap.md). Periksa branch dan working tree sebelum mengedit.

## Branch dan pull request

| Branch | Penggunaan |
| --- | --- |
| `main` | Baseline yang dapat diperiksa/dibangun ketika kode tersedia |
| `feat/<fitur-kecil>` | Kemampuan baru, misalnya `feat/open-shift` |
| `fix/<masalah>` | Koreksi perilaku, misalnya `fix/duplicate-payment` |
| `docs/<topik>` | Dokumen, misalnya `docs/product-requirements` |
| `chore/<topik>` | Tooling atau konfigurasi |

1. Mulai branch dari `main` yang sudah memuat dependensi tugas dan telah direview. Jangan meninggalkan perubahan pengguna.
2. Ambil satu bagian fitur dengan acceptance criteria. Branch dapat berlangsung beberapa hari; tidak perlu branch baru setiap tanggal.
3. Buat draft PR ketika remote tersedia. Gunakan template PR dan tautkan tugas/requirement.
4. Push hasil sesi. Jika remote belum ada, catat bahwa perubahan hanya tersimpan lokal.
5. Lakukan pemeriksaan yang sesuai dan minta review pemilik melalui penyerahan hasil, bukan menganggap PR otomatis disetujui.
6. Setelah review selesai, merge dengan **merge commit**. Hindari squash sebagai default agar riwayat kerja tetap tersimpan.

Gunakan nama branch berdasarkan pekerjaan, misalnya `docs/product-requirements` atau `feat/menu-catalog`. Bila pekerjaan bergantung pada branch yang belum diintegrasikan, catat dependensinya secara eksplisit.

## Commit

Format: `<type>: <hasil perubahan>`; gunakan Bahasa Inggris dan jelaskan perilaku yang berubah.

Contoh:

```text
docs: define cashier shift acceptance criteria
feat: reject payments on closed shifts
test: cover payment retries after response timeout
fix: preserve original recipe snapshot on refunds
```

Target harian 3–4 commit yang masing-masing merupakan perubahan utuh. Untuk fitur, pemisahannya dapat berupa kontrak/domain, implementasi, pengujian, dan perbaikan/dokumentasi. Untuk dokumen, pisahkan cakupan, aturan/penerimaan, dan hasil review bila memang berbeda.

Jumlah commit mengikuti pekerjaan nyata. Jangan membuat commit kosong, mengganti tanggal, atau menulis pengujian yang hanya menyalin implementasi untuk menambah aktivitas.

Stage file yang sudah diperiksa secara eksplisit. Tinjau diff sebelum commit agar tidak memasukkan secret, backup, atau data operasional.

## Definition of done

- Acceptance criteria tugas terpenuhi dan dependensinya jelas.
- Build/typecheck/lint/pengujian yang relevan lolos ketika tersedia; jelaskan pemeriksaan yang belum dapat dijalankan.
- Aturan bisnis, API, dan dokumen diperbarui bersama perubahan terkait.
- Transaksi kritis diuji untuk request berulang, konkurensi, rollback, dan otorisasi yang relevan.
- Status implementasi dibedakan dari status review dan deployment.
- Progres mencatat bukti, penghambat, dan tugas berikutnya.

Pada tahap fondasi, validasi terbatas pada dokumen dan Git. Tidak ada `package.json`, sehingga tidak ada perintah build/test aplikasi yang dapat dijalankan.

## Penggunaan GitHub

Remote yang dituju adalah `https://github.com/Tano11933/servio-pos.git`. Periksa remote dan akses sebelum push. Jangan menimpa remote yang sudah ada atau force-push branch bersama.

Email commit harus terhubung ke akun GitHub, termasuk bila memakai alamat `noreply` GitHub. Identitas lokal tersedia, tetapi keterkaitannya dengan akun belum diverifikasi. Commit menggunakan identitas pemilik repository tanpa atribusi tambahan.

Commit di feature branch belum tentu muncul di grafik kontribusi sebelum masuk ke default branch. Lihat [ketentuan kontribusi GitHub](https://docs.github.com/en/account-and-profile/reference/profile-contributions-reference). Merge setelah review; grafik kontribusi tidak menjadi alasan melewati pemeriksaan.

## Review pemilik dan handoff

Ringkasan sesi menyebutkan hasil, file/branch yang dapat diperiksa, cara mencoba, validasi, dan keputusan yang diperlukan. Pada tahap fondasi, review dilakukan melalui dokumen dan riwayat Git.

Catatan progres memuat milestone aktif, hasil, bukti validasi, batasan, dan pekerjaan berikutnya. Jangan menulis pesan commit, hash, atau status push yang belum benar-benar terjadi.

Repository dipublikasikan sebagai portfolio. Lisensi distribusi masih perlu diputuskan pemilik; tidak adanya file lisensi bukan klaim bahwa proyek berlisensi open source.
