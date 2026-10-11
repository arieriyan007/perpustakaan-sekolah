# 1. Alur Sistem

## Aktor

| Aktor | Tanggung jawab |
|---|---|
| Admin | Pengguna, master data, konfigurasi, dan laporan |
| Petugas | Anggota, buku, peminjaman, dan pengembalian |
| Siswa | Katalog dan riwayat peminjaman miliknya |
| Guru | Opsional; dapat diperlakukan sebagai anggota khusus |

## Alur umum

```text
Browser -> Login -> Cek hak akses -> Dashboard -> Fitur -> Logout
                         |
                         +-> gagal: pesan validasi / 403
```

## Alur peminjaman

1. Petugas memilih anggota.
2. Petugas memilih satu atau beberapa buku.
3. Sistem mengecek status anggota dan stok setiap buku.
4. Sistem menghitung tanggal jatuh tempo.
5. Sistem menyimpan transaksi dan detail buku.
6. Sistem mengurangi stok.
7. Sistem menampilkan nomor transaksi.

Jika satu buku gagal diproses, seluruh transaksi dibatalkan dengan database transaction.

## Alur pengembalian

1. Petugas mencari nomor transaksi atau anggota.
2. Sistem menampilkan buku yang masih dipinjam.
3. Petugas memilih buku yang dikembalikan.
4. Sistem mencatat tanggal kembali.
5. Sistem menghitung keterlambatan dan denda.
6. Sistem menambah stok.
7. Sistem memperbarui status detail dan status transaksi.

## Status

Transaksi: `active`, `partially_returned`, `returned`, `cancelled`.

Detail: `borrowed`, `returned`, `lost`, `damaged`.

## Aturan bisnis awal

- Anggota tidak aktif tidak boleh meminjam.
- Stok tidak boleh kurang dari nol.
- Jumlah buku per transaksi dibatasi konfigurasi.
- Denda dihitung jika tanggal kembali melewati jatuh tempo.
- Nomor transaksi harus unik.
- Buku yang sudah pernah dipinjam sebaiknya menggunakan soft delete.
