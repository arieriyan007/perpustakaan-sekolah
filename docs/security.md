# Security & Performance Rules

Dokumen ini berisi aturan keamanan dan efisiensi dasar untuk project perpustakaan sekolah.

## 1. Authentication dan authorization

- Semua halaman internal wajib menggunakan middleware `auth`.
- Hak akses harus diperiksa di server menggunakan middleware, policy, atau Form Request.
- Jangan hanya menyembunyikan tombol di Blade untuk membatasi akses.
- Role minimal: `admin`, `officer`, dan `student`.
- User nonaktif tidak boleh melakukan transaksi.
- User `student` tidak boleh mengakses panel Filament admin.
- User `admin` dan `officer` hanya mendapat aksi sesuai policy dan kebutuhan tugasnya.

Contoh posisi route: `routes/web.php`

```php
Route::middleware(['auth'])->group(function () {
    Route::resource('books', BookController::class);
});
```

## 2. Validasi dan input

- Semua input wajib divalidasi di server.
- Gunakan Form Request untuk validasi yang berulang atau kompleks.
- Jangan mempercayai nilai harga, stok, role, atau status dari browser.
- Gunakan Eloquent atau Query Builder, bukan query SQL yang digabung langsung dengan input user.
- Gunakan `{{ }}` di Blade agar output di-escape.
- Validasi pada Filament Form adalah lapisan UI; validasi dan authorization server tetap wajib.

## 3. Aturan akses Filament

Implementasikan `FilamentUser` dan `canAccessPanel()` pada `App\Models\User` setelah Filament dipasang. Contoh aturan awal:

```php
public function canAccessPanel(\Filament\Panel $panel): bool
{
    return $this->is_active
        && in_array($this->role, ['admin', 'officer'], true);
}
```

Aturan ini hanya membatasi akses panel. Aksi create, update, delete, dan akses record tetap harus dilindungi policy.

## 4. Perlindungan database

- Gunakan foreign key, unique index, dan constraint yang sesuai.
- Gunakan `DB::transaction()` untuk peminjaman dan pengembalian.
- Gunakan `lockForUpdate()` saat mengubah stok pada transaksi bersamaan.
- Jangan pernah membiarkan stok menjadi negatif.
- Simpan password melalui hashing Laravel; jangan menyimpan password plain text.
- Jangan commit `.env`, password, token, atau credential database.
- Jangan mengubah source code di `vendor/`; perubahan seperti itu tidak terkontrol dan dapat hilang saat dependency di-install ulang.

## 5. Hindari N+1 query

N+1 query harus dihindari. N+1 bukan pola yang perlu digunakan; ini terjadi ketika aplikasi menjalankan satu query utama lalu satu query tambahan untuk setiap data relasi.

### Jangan dilakukan

```php
$books = Book::all();

foreach ($books as $book) {
    echo $book->category->name;
}
```

### Gunakan eager loading

```php
$books = Book::query()
    ->with('category')
    ->paginate(15);
```

### Aturan query

- Gunakan `with()` untuk relasi yang akan ditampilkan.
- Gunakan `withCount()` untuk jumlah relasi, bukan menghitung di dalam loop.
- Pilih kolom yang diperlukan jika data besar.
- Gunakan pagination untuk daftar panjang.
- Periksa query dengan Laravel Debugbar atau Laravel Telescope hanya di environment development.
- Tambahkan feature test untuk halaman yang memiliki banyak relasi.

## 6. Session, CSRF, dan upload

- Form `POST`, `PUT`, `PATCH`, dan `DELETE` wajib menggunakan CSRF protection Laravel.
- Regenerate session setelah login jika flow autentikasi dikustomisasi.
- Batasi ukuran dan tipe file upload.
- Simpan file upload di storage, bukan langsung di folder yang dapat menjalankan script.
- Jangan menampilkan stack trace kepada user production.

## 7. Production checklist

- [ ] `APP_ENV=production`.
- [ ] `APP_DEBUG=false`.
- [ ] HTTPS aktif.
- [ ] Credential database tidak berada di repository.
- [ ] Backup database dijadwalkan dan pernah diuji restore.
- [ ] Log error tidak menampilkan data sensitif.
- [ ] Dependency diperiksa dan diperbarui secara terkontrol.
- [ ] Query halaman utama diperiksa dari N+1 query.
