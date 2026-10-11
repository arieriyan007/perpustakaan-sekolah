# Dokumentasi Project Perpustakaan Sekolah

Panduan membangun aplikasi perpustakaan sekolah berbasis web dengan Laravel 13, PHP, MySQL, Blade, dan Filament 5. Laravel menjadi framework utama, PHP menjadi bahasa backend, dan Filament menjadi panel admin. Semua contoh kode di dokumen ini dimasukkan manual oleh user ke posisi file yang disebutkan.

## Arsitektur teknologi

| Komponen | Peran |
|---|---|
| Laravel 13 | Framework utama backend dan routing |
| PHP 8.3+ | Bahasa pemrograman backend |
| MySQL | Database aplikasi |
| Filament 5 | Panel admin untuk admin dan petugas |
| Blade | Halaman publik dan halaman yang tidak membutuhkan panel admin |
| Tailwind CSS + Vite | Styling dan build asset frontend |

Filament digunakan sebagai lapisan panel admin, bukan pengganti Laravel. Model, migration, policy, service, dan aturan bisnis tetap berada di struktur Laravel.

## Urutan membaca

| Urutan | Dokumen | Isi |
|---|---|---|
| 1 | [01-alur-sistem.md](01-alur-sistem.md) | Aktor, fitur, dan alur proses |
| 2 | [desain.md](desain.md) | Desain visual, layout, dan komponen UI |
| 3 | [02-desain-sistem.md](02-desain-sistem.md) | Arsitektur halaman, Filament, dan navigasi |
| 4 | [03-roadmap.md](03-roadmap.md) | Tahapan pembangunan dan target |
| 5 | [04-database.md](04-database.md) | Tabel, relasi, dan rancangan data |
| 6 | [05-setup-project.md](05-setup-project.md) | Persiapan Laravel, PHP, MySQL, dan Git |
| 7 | [06-panduan-coding.md](06-panduan-coding.md) | Posisi file dan contoh coding manual |
| 8 | [07-standar-dokumentasi.md](07-standar-dokumentasi.md) | Dokumentasi, clean code, dan testing |
| 9 | [security.md](security.md) | Keamanan, validasi, authorization, dan query |
| 10 | [08-checklist.md](08-checklist.md) | Checklist implementasi dan rilis |

## Scope versi pertama

- Login dan logout.
- Hak akses admin, petugas, dan siswa.
- Kategori, buku, anggota, dan stok.
- Peminjaman, pengembalian, keterlambatan, dan denda.
- Dashboard dan laporan.

## Aturan kerja

- Jangan menulis query database langsung di Blade.
- Validasi input menggunakan Form Request.
- Gunakan migration untuk perubahan struktur database.
- Gunakan route name, bukan URL hard-code di view.
- Gunakan service untuk proses bisnis yang mulai kompleks.
- Gunakan Filament untuk panel admin dan petugas; gunakan Blade untuk halaman publik atau kebutuhan khusus.
- Jangan menaruh aturan bisnis hanya di konfigurasi tampilan Filament; gunakan policy, request, service, dan model Laravel.
- Jangan pernah mengubah, mengedit, atau menaruh custom code di dalam `vendor/` Laravel maupun Filament.
- Route harus clean code, rapi, memiliki nama route, middleware yang jelas, dan dokumentasi setiap perubahan.
- Setiap fitur wajib memiliki dokumentasi singkat dan skenario pengujian.
- Jangan menyimpan password atau kredensial database di Git.

## Dokumentasi fitur

Dokumentasi detail setiap fitur disimpan di [docs/features](features/). Setiap coding baru wajib memiliki catatan tujuan, posisi file, kode, cara menjalankan, dan cara testing.

Fitur aktif: [Authentication Aplikasi Perpustakaan](features/authentication.md).

Arsitektur panel: [Panel Admin Filament](features/filament-admin.md).
