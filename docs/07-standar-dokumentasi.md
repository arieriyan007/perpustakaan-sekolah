# 7. Standar Dokumentasi dan Clean Code

## Dokumentasi setiap fitur

Simpan fitur besar di `docs/features/<nama-fitur>.md` menggunakan format berikut:

```text
Nama fitur:
Tujuan:
Aktor yang dapat menggunakan:
Route:
Controller:
Request/validasi:
Model/tabel:
Alur sukses:
Alur gagal:
Skenario testing:
Catatan perubahan:
```

Setiap script atau coding baru wajib didokumentasikan sebelum lanjut ke fitur berikutnya. Minimal dokumentasi harus mencantumkan:

- Tujuan perubahan.
- Posisi file.
- Perintah generator atau instalasi jika ada.
- Kode yang dimasukkan.
- Cara menjalankan.
- Cara menguji.
- Status pekerjaan dan catatan error.

Contoh dokumentasi implementasi tersedia di [features/user-role.md](features/user-role.md).

## Penamaan

- Class: `PascalCase`, contoh `LoanService`.
- Method dan variable: `camelCase`, contoh `calculateFine`.
- Tabel dan kolom: `snake_case`, contoh `loan_items`.
- Route name: dot notation, contoh `loans.store`.

## Clean code

- Satu method memiliki satu tanggung jawab utama.
- Nama variable harus menjelaskan isi atau tujuan.
- Hindari magic number dan status string yang tersebar.
- Hindari duplikasi query.
- Gunakan dependency injection.
- Controller tipis; view hanya menampilkan data.
- Tangani exception dengan pesan aman dan log detail untuk developer.

## Commit

```text
feat: add book management
fix: prevent negative book stock
refactor: move loan logic to service
test: add loan return scenarios
docs: update database design
chore: update dependencies
```

## Testing minimum

Unit test untuk perhitungan denda, jatuh tempo, dan nomor transaksi.

Feature test untuk login, hak akses, pembuatan peminjaman, stok berkurang, pengembalian, stok bertambah, buku tanpa stok, dan denda keterlambatan.

## Keamanan minimum

- Gunakan autentikasi dan CSRF Laravel.
- Lindungi endpoint dengan middleware atau policy.
- Jangan percaya input dari browser.
- Gunakan Eloquent atau query builder.
- Jangan menampilkan stack trace pada production.
- Simpan rahasia di environment variable.
- Batasi upload berdasarkan MIME type, ukuran, dan lokasi penyimpanan.
