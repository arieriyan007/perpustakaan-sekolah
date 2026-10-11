# Desain Aplikasi

Desain dibuat sederhana, profesional, dan nyaman digunakan oleh admin, petugas, maupun siswa.

## 1. Arah visual

```text
Gaya       : bersih, tenang, modern
Warna utama: navy / biru tua
Warna aksen: biru muda atau emerald
Latar      : putih dan abu-abu sangat muda
Font       : sans-serif yang mudah dibaca
Radius     : sedang, tidak berlebihan
Bayangan   : tipis, hanya untuk kartu dan modal
```

Gunakan maksimal satu warna utama, satu warna aksen, dan warna status. Hindari terlalu banyak warna agar aplikasi terlihat rapi.

## 2. Layout utama

```text
┌──────────────────────────────────────────────┐
│ Logo Perpustakaan              Profil ▾      │
├───────────────┬──────────────────────────────┤
│ Dashboard     │ Judul halaman                 │
│ Data Master   │ Deskripsi singkat              │
│ Transaksi     │                                │
│ Laporan       │ ┌──────┐ ┌──────┐ ┌──────┐    │
│               │ │ Buku │ │Aktif │ │Telat │    │
│               │ └──────┘ └──────┘ └──────┘    │
│               │                                │
│               │ Tabel atau konten utama        │
└───────────────┴──────────────────────────────┘
```

Gunakan sidebar untuk desktop dan menu drawer untuk layar kecil. Konten utama harus memiliki ruang kosong yang cukup agar tidak terlihat padat.

## 3. Dashboard

Tampilkan hanya informasi yang membantu pekerjaan harian:

- Total buku.
- Stok tersedia.
- Anggota aktif.
- Peminjaman aktif.
- Buku terlambat.
- Lima transaksi terbaru.

Kartu statistik menggunakan angka besar, label pendek, dan ikon sederhana. Hindari grafik yang tidak membantu keputusan.

## 4. Halaman daftar data

Setiap halaman daftar mengikuti pola yang sama:

1. Breadcrumb kecil.
2. Judul dan deskripsi singkat.
3. Tombol aksi utama di kanan atas.
4. Search dan filter.
5. Tabel dengan kolom penting saja.
6. Pagination.

Contoh judul halaman:

```text
Buku
Kelola koleksi dan stok buku perpustakaan.
```

## 5. Form

- Kelompokkan input yang berhubungan.
- Gunakan label yang jelas.
- Tandai field wajib dengan `*`.
- Tampilkan error tepat di bawah field.
- Gunakan tombol utama dan tombol batal yang jelas.
- Jangan membuat form terlalu panjang dalam satu layar jika dapat dibagi menjadi beberapa bagian.

## 6. Status dan feedback

| Status | Warna | Contoh |
|---|---|---|
| Berhasil | Hijau | Buku berhasil disimpan |
| Informasi | Biru | Belum ada transaksi hari ini |
| Peringatan | Kuning | Stok buku hampir habis |
| Gagal | Merah | Buku tidak tersedia |
| Netral | Abu-abu | Belum diproses |

Pesan harus menjelaskan apa yang terjadi dan, bila perlu, apa yang harus dilakukan user.

## 7. Prinsip tampilan

- Satu halaman memiliki satu fokus utama.
- Gunakan whitespace agar tampilan tidak penuh.
- Gunakan istilah yang konsisten.
- Jangan mengandalkan warna saja untuk menyampaikan status.
- Gunakan tabel pada data administratif dan kartu pada ringkasan.
- Pastikan tombol memiliki teks atau label aksesibilitas.
- Uji tampilan pada laptop dan ponsel.

## 8. Komponen yang disarankan

Simpan komponen berulang di `resources/views/components/`:

```text
resources/views/components/
├── alert.blade.php
├── badge.blade.php
├── button.blade.php
├── card-stat.blade.php
├── input-error.blade.php
├── modal.blade.php
└── pagination.blade.php
```

Dengan komponen ini, halaman buku, anggota, dan transaksi tetap konsisten serta lebih mudah dirawat.
