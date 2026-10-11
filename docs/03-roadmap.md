# 3. Roadmap Pembangunan

## Fase 0 — Perencanaan

- Tetapkan aktor, scope, alur, status, dan aturan denda.
- Output: dokumen alur dan desain sistem.

## Fase 1 — Persiapan environment dan dependensi

Selesaikan persiapan komputer sebelum membuat fitur aplikasi.

### Software wajib

- PHP sesuai versi yang didukung oleh versi Laravel yang dipilih.
- Composer untuk mengelola dependensi PHP.
- MySQL untuk database.
- Node.js dan npm untuk dependensi frontend serta Vite.
- Git untuk version control.
- Editor kode seperti VS Code.

### Verifikasi instalasi

Jalankan perintah berikut dan pastikan semuanya menampilkan versi tanpa error:

```bash
php -v
composer -V
mysql --version
node -v
npm -v
git --version
```

### Membuat project Laravel

```bash
composer create-project laravel/laravel perpustakaan
cd perpustakaan
php artisan key:generate
```

Jika menggunakan installer Laravel global, alternatifnya:

```bash
composer global require laravel/installer
laravel new perpustakaan
cd perpustakaan
```

Pilih database MySQL saat konfigurasi project jika installer menanyakannya.

### Memasang dependensi project

Dependensi PHP dipasang oleh Composer, sedangkan dependensi frontend dipasang oleh npm:

```bash
composer install
npm install
```

Dependensi tambahan dipasang hanya jika memang dibutuhkan. Contoh autentikasi Laravel berbasis Blade:

```bash
composer require laravel/breeze --dev
php artisan breeze:install blade
npm install
npm run build
```

Jangan memasang package tanpa alasan yang terdokumentasi. Catat package baru pada dokumentasi fitur atau changelog.

### Konfigurasi environment

- Salin `.env.example` menjadi `.env`.
- Isi nama database, username, dan password MySQL.
- Jalankan `php artisan key:generate`.
- Jangan commit file `.env`.

### Kriteria selesai fase

- `php artisan --version` berhasil.
- `php artisan serve` dapat membuka aplikasi.
- `npm run dev` atau `npm run build` berhasil.
- File `.env` sudah terisi tanpa credential masuk Git.
- Project sudah memiliki commit awal.

## Fase 2 — Setup project dan database

- Koneksi MySQL berhasil.
- Migration bawaan Laravel berjalan.
- Git dan authentication dasar siap.
- Database `perpustakaan_sekolah` sudah dibuat.

## Fase 3 — Database dan model

- Migration tabel inti selesai.
- Seeder role dan data awal tersedia.
- Relasi Eloquent diuji.

## Fase 4 — Master data

- CRUD kategori, buku, dan anggota.
- Search, filter, pagination, dan validasi.
- Data dapat dinonaktifkan atau di-soft-delete.

## Fase 5 — Transaksi inti

- Pembuatan peminjaman.
- Pengembalian sebagian atau seluruh buku.
- Perhitungan stok, keterlambatan, dan denda.
- Rollback jika sebagian proses gagal.

## Fase 6 — Dashboard dan laporan

- Statistik ringkas.
- Laporan berdasarkan rentang tanggal.
- Export CSV/PDF sebagai pengembangan berikutnya.

## Fase 7 — Hardening

- Authorization melalui middleware atau policy.
- Feature test untuk alur kritis.
- Logging, backup, review keamanan, dan performa.

## Prioritas fitur

| Prioritas | Fitur |
|---|---|
| P0 | Login, role, buku, anggota, peminjaman, pengembalian |
| P1 | Dashboard, laporan, pencarian, denda |
| P2 | Notifikasi, export PDF, barcode/QR, reservasi |
| P3 | Mobile, integrasi akademik, rekomendasi buku |
