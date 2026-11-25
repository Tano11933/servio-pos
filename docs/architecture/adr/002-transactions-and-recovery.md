# ADR-002: transaksi, idempotency, dan pemulihan

Status: baseline rancangan; wajib diuji dengan PostgreSQL saat implementasi. Mendukung BR-01–10, PAY-03/04, ORD-03/04, SHF-02, RET-01, dan ACC-02/03.

## Keputusan transaksi

Gunakan transaksi pendek READ COMMITTED dengan row lock eksplisit dan constraint unik/check. Semua read/write terkait lewat konteks transaksi yang sama; SQL terparameterisasi digunakan jika ORM belum mengekspresikan locking. Tidak ada panggilan printer, jaringan pembayaran, atau proses lambat di dalam transaksi. SERIALIZABLE seluruh aplikasi tidak dipilih sebagai pengganti pemodelan invariant.

Urutan lock global: sesi → membership → assignment (shared untuk command; pencabutan memakai exclusive), claim Operation → shift → order → payment → item/unit/reservasi → saldo ingredient terurut ID. Lewati kelompok yang tidak diperlukan; urutkan ID dalam kelompok. Transaksi yang hanya menulis stok tidak kemudian mengambil lock order. Tidak ada jalur close-shift yang mengunci shift lalu memanggil transaksi baru.

Pencabutan sesi/membership harus memakai urutan akses yang sama. Operasi yang sudah memegang shared lock boleh selesai sebelum pencabutan; setelah pencabutan commit, command berikutnya ditolak. Ini mendefinisikan urutan konsisten, bukan membatalkan transaksi yang sudah berjalan.

Perintah mengunci agregat, membandingkan expected version, membaca ulang saldo/status, lalu menulis seluruh efek dan menaikkan versi. Pembayaran mengambil shift sebelum order; refund membaca ID sumber untuk menentukan shift/order lalu memvalidasinya lagi setelah lock. Sumber tidak boleh diganti di luar lock. Unique order payment, meja aktif, shift aktif dan refund unit menjadi pagar tambahan. Pembuatan agregat baru memakai unique constraint, bukan mengandalkan lock row yang belum ada; insert-on-conflict untuk saldo ingredient dilanjutkan lock row sebelum mutasi.

## Idempotency persisten

Scope key = tenant, outlet, actor, key. Canonical request hash menyertakan command, path/resource, expected version dan normalized body; selector scope tidak pernah dipercaya tanpa izin. Key dipakai ulang untuk operasi yang sama sampai hasil diketahui.

Di dalam transaksi: verifikasi izin terkini, insert Operation unik, jalankan command, simpan resource ID/status/response sukses, lalu commit semuanya bersama. Insert bersamaan atas key sama menunggu konflik unik atau timeout; pihak kalah membaca hasil yang sudah commit dalam transaksi baru setelah memeriksa izin. Hash berbeda = conflict. Hasil sukses tidak boleh terpisah dari mutasi bisnis.

Jika command ditolak, rollback seluruhnya termasuk claim; respons penolakan menyatakan operasi tidak commit. Tidak menyimpan status IN_PROGRESS permanen yang dapat tertinggal setelah crash. Jika lock timeout sebelum hasil diketahui, klien melihat status belum diketahui, memakai lookup dan retry key sama. Lookup tidak ditemukan belum membuktikan operasi lain yang sedang berjalan sudah gagal.

Simpan record sukses selama masa retensi transaksi pilot; tidak membersihkan key secara TTL pendek yang memungkinkan retry lama menggandakan mutasi. Restore database mengembalikan operasi dan mutasi pada titik pemulihan yang sama. Kehilangan data setelah RPO tetap memerlukan rekonsiliasi uang eksternal; idempotency bukan jaminan terhadap data yang belum ada di backup.

## Batas retry dan kegagalan

Deadlock/serialization failure yang PostgreSQL pastikan rollback dapat diulang server maksimal 3 percobaan dengan jitter, menggunakan key/expected version sama. Setelah batas, kembalikan error retryable. Konflik versi/bisnis tidak dicoba ulang diam-diam. Koneksi putus saat COMMIT memiliki hasil ambigu: jangan menjalankan command baru; lookup key di koneksi baru, lalu pulihkan hasil atau minta retry key sama.

Audit, ledger, reservation, cash dan outcome Operation ditulis dalam transaksi bisnis yang sama. Dapur membaca snapshot DB; tidak perlu event bus untuk menjamin tiket muncul. Notifikasi/integrasi eksternal nanti memerlukan transactional outbox dan consumer dedup tersendiri sebelum ditambahkan.

## Bukti yang diwajibkan

Barrier tests dua koneksi nyata untuk key sama/berbeda, dua kasir, last stock, meja/shift aktif, refund unit, pencabutan akses, dan close-shift. Fault injection sebelum commit, sesudah commit sebelum respons, serta restart setelah sukses. Mock atau unit test saja tidak membuktikan keputusan ini.

Referensi: [locking PostgreSQL](https://www.postgresql.org/docs/current/explicit-locking.html), [constraints PostgreSQL](https://www.postgresql.org/docs/current/ddl-constraints.html), [transaksi Prisma](https://www.prisma.io/docs/orm/prisma-client/queries/transactions).
