# 2. Desain Sistem

## Arsitektur teknologi

```text
Laravel 13
├── PHP backend
├── Eloquent ORM + MySQL
├── Filament 5 panel admin
│   ├── Admin
│   ├── Petugas
│   └── Resources, Pages, Widgets
└── Blade + Tailwind untuk halaman publik dan kebutuhan khusus
```

Laravel tetap menjadi pusat aplikasi. Filament hanya menjadi antarmuka admin di atas model, policy, service, dan database Laravel.

## Arsitektur request

```text
Browser
  ├── /admin -> Filament Panel -> Resource/Page -> Policy/Service -> Model -> MySQL
  └── /      -> Route -> Middleware -> Controller -> Form Request
                                      -> Service -> Model -> MySQL
                                   <- Blade View <- Controller
```

Controller menerima request dan mengembalikan response. Form Request menangani validasi. Service menangani aturan bisnis. Model menangani relasi dan akses data. Filament Resource menangani tabel dan form panel, tetapi aturan authorization tetap wajib berada di policy atau mekanisme server-side.

## Pembagian penggunaan

| Area | Teknologi | Pengguna |
|---|---|---|
| Panel `/admin` | Filament | Admin dan petugas |
| Katalog publik | Blade + Laravel | Siswa, guru, pengunjung |
| Proses bisnis | Service Laravel | Dipakai oleh Filament dan Blade |
| Data dan relasi | Eloquent + MySQL | Seluruh aplikasi |

## Rekomendasi arsitektur

- Gunakan Filament untuk CRUD, dashboard, tabel, filter, form, dan laporan internal.
- Gunakan Blade untuk katalog publik dan halaman yang membutuhkan desain sangat khusus.
- Gunakan Service untuk stok, peminjaman, pengembalian, dan denda agar logika tidak terkunci di Filament Resource.
- Gunakan Policy untuk membatasi aksi admin, petugas, dan siswa.
- Jangan membuat panel Filament kedua sebelum kebutuhan multi-panel benar-benar jelas.

## Struktur halaman

```text
/
├── /login
├── /dashboard
├── /books
├── /categories
├── /members
├── /loans
├── /returns
├── /reports
└── /users
```

## Menu navigasi

1. Dashboard
2. Data Master: Buku, Kategori, Anggota
3. Transaksi: Peminjaman, Pengembalian
4. Laporan
5. Pengguna, khusus admin
6. Profil dan logout

Menu harus mengikuti hak akses. Jangan hanya menyembunyikan tombol; endpoint juga wajib dilindungi middleware atau policy.

## Isi dashboard

- Jumlah judul buku.
- Stok tersedia.
- Anggota aktif.
- Peminjaman aktif.
- Buku terlambat.
- Peminjaman terbaru.

## Prinsip UI

- Gunakan bahasa Indonesia yang konsisten.
- Setiap form memiliki label dan pesan validasi.
- Gunakan konfirmasi sebelum aksi berisiko.
- Gunakan pagination, search, dan filter pada data panjang.
- Tampilkan status dengan label yang mudah dibaca.
- Pastikan tampilan nyaman di laptop dan ponsel.

## Nama route yang disarankan

| Fitur | Route name |
|---|---|
| Dashboard | `dashboard` |
| Buku | `books.index`, `books.create`, `books.store`, `books.edit`, `books.update` |
| Kategori | `categories.index`, `categories.store`, `categories.update` |
| Anggota | `members.index`, `members.store`, `members.update` |
| Peminjaman | `loans.index`, `loans.create`, `loans.store`, `loans.show` |
| Pengembalian | `returns.store` |
| Laporan | `reports.loans`, `reports.returns` |
