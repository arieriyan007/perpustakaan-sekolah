# 2. Desain Sistem

## Arsitektur

```text
Browser
  -> Route -> Middleware -> Controller -> Form Request
                                      -> Service -> Model -> MySQL
  <- Blade View <- Controller
```

Controller menerima request dan mengembalikan response. Form Request menangani validasi. Service menangani aturan bisnis. Model menangani relasi dan akses data.

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
