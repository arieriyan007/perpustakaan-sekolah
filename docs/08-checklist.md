# 8. Checklist Implementasi dan Rilis

## Perencanaan

- [ ] Aktor sistem disepakati.
- [ ] Alur peminjaman disepakati.
- [ ] Aturan denda disepakati.
- [ ] Desain tabel disepakati.
- [ ] Versi PHP, Laravel, MySQL, dan Node.js dicatat.

## Setup

- [ ] Project Laravel berhasil dibuat.
- [ ] `.env` dibuat dari `.env.example`.
- [ ] Database MySQL berhasil dibuat.
- [ ] `php artisan migrate` berhasil.
- [ ] `php artisan serve` berhasil.
- [ ] Git sudah diinisialisasi.

## Database dan master data

- [ ] Migration semua tabel inti selesai.
- [ ] Seeder role dan data contoh selesai.
- [ ] Foreign key dan index diperiksa.
- [ ] CRUD kategori, buku, dan anggota selesai.
- [ ] Validasi, search, filter, dan pagination selesai.

## Transaksi

- [ ] Peminjaman selesai.
- [ ] Stok berkurang dengan benar.
- [ ] Buku tanpa stok ditolak.
- [ ] Pengembalian selesai.
- [ ] Stok bertambah dengan benar.
- [ ] Denda dan status transaksi benar.
- [ ] Rollback database diuji.

## Quality gate

- [ ] `php artisan test` berhasil.
- [ ] Tidak ada query database di Blade.
- [ ] Tidak ada password di repository.
- [ ] Hak akses diuji sebagai admin, petugas, dan siswa.
- [ ] Dokumentasi fitur diperbarui.
- [ ] Commit konsisten.

## Production

- [ ] `APP_DEBUG=false`.
- [ ] HTTPS aktif.
- [ ] Backup database diuji.
- [ ] Storage dan permission diperiksa.
- [ ] Log dan monitoring tersedia.
- [ ] Prosedur rollback disiapkan.
