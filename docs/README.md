# Dokumentasi Project Perpustakaan Sekolah

Panduan membangun aplikasi perpustakaan sekolah berbasis web dengan Laravel, PHP, MySQL, dan Blade. Semua contoh kode di dokumen ini dimasukkan manual oleh user ke posisi file yang disebutkan.

## Urutan membaca

| Urutan | Dokumen | Isi |
|---|---|---|
| 1 | [01-alur-sistem.md](01-alur-sistem.md) | Aktor, fitur, dan alur proses |
| 2 | [desain.md](desain.md) | Desain visual, layout, dan komponen UI |
| 3 | [02-desain-sistem.md](02-desain-sistem.md) | Arsitektur halaman dan navigasi detail |
| 4 | [03-roadmap.md](03-roadmap.md) | Tahapan pembangunan dan target |
| 5 | [04-database.md](04-database.md) | Tabel, relasi, dan rancangan data |
| 6 | [05-setup-project.md](05-setup-project.md) | Persiapan Laravel, PHP, MySQL, dan Git |
| 7 | [06-panduan-coding.md](06-panduan-coding.md) | Posisi file dan contoh coding manual |
| 8 | [07-standar-dokumentasi.md](07-standar-dokumentasi.md) | Dokumentasi, clean code, dan testing |
| 9 | [security.md](security.md) | Keamanan, validasi, dan efisiensi query |
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
- Setiap fitur wajib memiliki dokumentasi singkat dan skenario pengujian.
- Jangan menyimpan password atau kredensial database di Git.

## Dokumentasi fitur

Dokumentasi detail setiap fitur disimpan di [docs/features](features/). Setiap coding baru wajib memiliki catatan tujuan, posisi file, kode, cara menjalankan, dan cara testing.
