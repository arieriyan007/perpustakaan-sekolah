# 8. Checklist Implementasi dan Rilis

## Perencanaan

- [ ] Aktor sistem disepakati.
- [ ] Alur peminjaman disepakati.
- [ ] Aturan denda disepakati.
- [ ] Desain tabel disepakati.
- [ ] Versi PHP, Laravel, MySQL, dan Node.js dicatat.

## Setup

- [ ] Project Laravel berhasil dibuat.
- [ ] Versi Laravel 13, PHP, MySQL, Node.js, dan npm dicatat.
- [ ] Filament 5 dipasang sebagai panel admin.
- [ ] Panel `/admin` dapat dibuka.
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
- [ ] Category Resource Filament selesai.
- [ ] Book Resource Filament selesai.
- [ ] Member Resource Filament selesai.
- [ ] Validasi, search, filter, dan pagination selesai.

## Transaksi

- [ ] Peminjaman selesai.
- [ ] Loan Resource Filament selesai.
- [ ] Stok berkurang dengan benar.
- [ ] Buku tanpa stok ditolak.
- [ ] Pengembalian selesai.
- [ ] Stok bertambah dengan benar.
- [ ] Denda dan status transaksi benar.
- [ ] Rollback database diuji.

## Quality gate

- [ ] `php artisan test` berhasil.
- [ ] `routes/web.php` dan `routes/api.php` tidak berisi logika bisnis.
- [ ] Semua route penting memiliki route name dan middleware yang sesuai.
- [ ] Setiap perubahan route sudah didokumentasikan.
- [ ] `php artisan route:list` sudah diperiksa.
- [ ] Tidak ada query database di Blade.
- [ ] Tidak ada password di repository.
- [ ] Tidak ada file `vendor/` yang diedit atau dimodifikasi manual.
- [ ] Hak akses diuji sebagai admin, petugas, dan siswa.
- [ ] Student ditolak dari panel Filament.
- [ ] Policy diuji untuk create, update, dan delete.
- [ ] Query Resource diperiksa dari N+1.
- [ ] Dokumentasi fitur diperbarui.
- [ ] Commit konsisten.

## Production

- [ ] `APP_DEBUG=false`.
- [ ] HTTPS aktif.
- [ ] Backup database diuji.
- [ ] Storage dan permission diperiksa.
- [ ] Log dan monitoring tersedia.
- [ ] Prosedur rollback disiapkan.
