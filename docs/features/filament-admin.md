# Fitur: Panel Admin Filament

## Keputusan arsitektur

Project menggunakan:

- Laravel 13 sebagai framework utama.
- PHP sebagai bahasa backend.
- MySQL sebagai database.
- Filament 5 sebagai panel admin untuk admin dan petugas.
- Blade sebagai halaman publik atau halaman khusus di luar panel.

Filament tidak menggantikan Laravel. Migration, model, policy, service, testing, dan aturan bisnis tetap mengikuti struktur Laravel.

## Aturan vendor

Source code Laravel, Filament, Livewire, dan package Composer berada di `vendor/` dan tidak boleh diedit atau diotak-atik. Customization harus dilakukan melalui konfigurasi, provider, policy, resource, service, view, atau extension point resmi.

## Alasan penggunaan Filament

- Mempercepat pembuatan CRUD internal.
- Menyediakan form, table, filter, action, widget, dan dashboard.
- Cocok untuk data kategori, buku, anggota, dan transaksi petugas.
- Tetap dapat dikustomisasi dengan struktur aplikasi Laravel.

## Batasan penggunaan

- Panel Filament digunakan untuk admin dan petugas.
- Siswa dan pengunjung menggunakan halaman publik Blade.
- Proses stok, peminjaman, pengembalian, dan denda tidak boleh hanya berada di Resource Filament.
- Logika proses tersebut harus diletakkan di Service agar dapat dipakai ulang dari Filament, command, atau halaman publik.

## Instalasi manual

Jalankan dari root project, yaitu folder yang memiliki file `artisan`:

```bash
composer require filament/filament:"^5.0"
php artisan filament:install --panels
npm install
npm run build
```

Provider yang diharapkan:

```text
app/Providers/Filament/AdminPanelProvider.php
```

Panel dibuka melalui:

```text
http://127.0.0.1:8000/admin
```

## Akses panel berdasarkan role

Posisi file: `app/Models/User.php`.

Setelah Filament terpasang, implementasikan kontrak Filament dan pembatasan panel:

```php
use Filament\Models\Contracts\FilamentUser;
use Filament\Panel;

class User extends Authenticatable implements FilamentUser
{
    public function canAccessPanel(Panel $panel): bool
    {
        return $this->is_active
            && in_array($this->role, ['admin', 'officer'], true);
    }
}
```

## Resource yang akan dibuat

```text
app/Filament/Resources/
├── CategoryResource.php
├── BookResource.php
├── MemberResource.php
└── LoanResource.php
```

Perintah contoh:

```bash
php artisan make:filament-resource Category
php artisan make:filament-resource Book
php artisan make:filament-resource Member
php artisan make:filament-resource Loan
```

## Testing minimum

- Admin dapat membuka `/admin`.
- Officer dapat membuka `/admin`.
- Student ditolak dari `/admin`.
- User nonaktif ditolak dari `/admin`.
- Policy membatasi aksi create, update, dan delete.
- Resource tidak menghasilkan N+1 query.

## Usulan pengembangan

Filament sebaiknya dipakai sebagai panel operasional, bukan sebagai satu-satunya tempat seluruh aplikasi. Pendekatan hybrid Filament + Blade memberi admin UI yang cepat dan tetap memberi kebebasan untuk membuat katalog publik yang lebih ringan serta sesuai desain sekolah.

## Status

- [ ] Filament dipasang.
- [ ] Akses panel berdasarkan role selesai.
- [ ] Category Resource selesai.
- [ ] Book Resource selesai.
- [ ] Member Resource selesai.
- [ ] Loan Resource selesai.
