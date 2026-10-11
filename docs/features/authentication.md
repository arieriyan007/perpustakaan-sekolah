# Fitur: Authentication Aplikasi Perpustakaan

## Tujuan

Menyediakan login, logout, session regeneration, dan pembatasan login untuk user yang tidak aktif.

Untuk panel admin dan petugas, kita akan menggunakan autentikasi panel Filament di `/admin`. Authentication Blade pada dokumen ini digunakan bila aplikasi membutuhkan halaman login publik atau alur login khusus di luar panel Filament.

Implementasi tahap ini menggunakan authentication bawaan Laravel berbasis session dan Blade. Starter kit Laravel bersifat opsional; dokumentasi resmi juga menyediakan pilihan starter kit jika ingin scaffolding otomatis. [Laravel Authentication](https://laravel.com/docs/13.x/authentication)

## File yang akan dibuat

```text
app/Http/Controllers/Auth/LoginController.php
app/Http/Requests/LoginRequest.php
resources/views/auth/login.blade.php
resources/views/layouts/app.blade.php
resources/views/dashboard.blade.php
routes/web.php
```

## Perintah generator

```bash
php artisan make:request LoginRequest
php artisan make:controller Auth/LoginController
mkdir -p resources/views/auth resources/views/layouts
```

## 1. Form Request

Posisi file: `app/Http/Requests/LoginRequest.php`

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class LoginRequest extends FormRequest
{
    public function authorize(): bool
    {
        return true;
    }

    public function rules(): array
    {
        return [
            'email' => ['required', 'email'],
            'password' => ['required', 'string'],
            'remember' => ['sometimes', 'boolean'],
        ];
    }
}
```

## 2. Login Controller

Posisi file: `app/Http/Controllers/Auth/LoginController.php`

```php
<?php

namespace App\Http\Controllers\Auth;

use App\Http\Controllers\Controller;
use App\Http\Requests\LoginRequest;
use Illuminate\Http\RedirectResponse;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Auth;
use Illuminate\View\View;

class LoginController extends Controller
{
    public function create(): View
    {
        return view('auth.login');
    }

    public function store(LoginRequest $request): RedirectResponse
    {
        $data = $request->validated();
        $remember = (bool) ($data['remember'] ?? false);

        unset($data['remember']);
        $data['is_active'] = true;

        if (! Auth::attempt($data, $remember)) {
            return back()
                ->withErrors(['email' => 'Email atau password salah.'])
                ->onlyInput('email');
        }

        $request->session()->regenerate();

        return to_route('dashboard');
    }

    public function destroy(Request $request): RedirectResponse
    {
        Auth::logout();

        $request->session()->invalidate();
        $request->session()->regenerateToken();

        return to_route('login');
    }
}
```

## 3. Routes

Posisi file: `routes/web.php`

```php
<?php

use App\Http\Controllers\Auth\LoginController;
use Illuminate\Support\Facades\Route;

Route::middleware('guest')->group(function () {
    Route::get('/login', [LoginController::class, 'create'])->name('login');
    Route::post('/login', [LoginController::class, 'store'])->name('login.store');
});

Route::middleware('auth')->group(function () {
    Route::get('/', fn () => to_route('dashboard'));
    Route::view('/dashboard', 'dashboard')->name('dashboard');
    Route::post('/logout', [LoginController::class, 'destroy'])->name('logout');
});
```

## 4. Layout

Posisi file: `resources/views/layouts/app.blade.php`

```blade
<!doctype html>
<html lang="id">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>@yield('title', 'Aplikasi Perpustakaan')</title>
    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
<body class="min-h-screen bg-slate-100 text-slate-900">
    <main class="mx-auto max-w-7xl p-6">
        @yield('content')
    </main>
</body>
</html>
```

## 5. Halaman login

Posisi file: `resources/views/auth/login.blade.php`

```blade
@extends('layouts.app')

@section('title', 'Login Aplikasi Perpustakaan')

@section('content')
    <div class="mx-auto mt-16 max-w-md rounded-2xl bg-white p-8 shadow-sm">
        <h1 class="text-2xl font-bold">Login Aplikasi Perpustakaan</h1>
        <p class="mt-2 text-sm text-slate-500">Masuk untuk mengelola perpustakaan.</p>

        @if ($errors->any())
            <div class="mt-4 rounded-lg bg-red-50 p-3 text-sm text-red-700">
                {{ $errors->first() }}
            </div>
        @endif

        <form method="POST" action="{{ route('login.store') }}" class="mt-6 space-y-4">
            @csrf

            <div>
                <label for="email" class="block text-sm font-medium">Email</label>
                <input id="email" name="email" type="email" value="{{ old('email') }}" required autofocus
                    class="mt-1 w-full rounded-lg border-slate-300">
            </div>

            <div>
                <label for="password" class="block text-sm font-medium">Password</label>
                <input id="password" name="password" type="password" required
                    class="mt-1 w-full rounded-lg border-slate-300">
            </div>

            <label class="flex items-center gap-2 text-sm">
                <input name="remember" type="checkbox" value="1">
                Ingat saya
            </label>

            <button type="submit" class="w-full rounded-lg bg-slate-900 px-4 py-2 font-semibold text-white">
                Masuk
            </button>
        </form>
    </div>
@endsection
```

## 6. Dashboard

Posisi file: `resources/views/dashboard.blade.php`

```blade
@extends('layouts.app')

@section('title', 'Dashboard Aplikasi Perpustakaan')

@section('content')
    <div class="flex items-center justify-between">
        <div>
            <p class="text-sm text-slate-500">Selamat datang, {{ auth()->user()->name }}</p>
            <h1 class="text-3xl font-bold">Dashboard Aplikasi Perpustakaan</h1>
        </div>

        <form method="POST" action="{{ route('logout') }}">
            @csrf
            <button type="submit" class="rounded-lg bg-red-600 px-4 py-2 text-white">Logout</button>
        </form>
    </div>
@endsection
```

## Cara menjalankan dan menguji

```bash
npm run build
php artisan route:list
php artisan test
php artisan serve
```

Login menggunakan akun seeder:

```text
admin@perpustakaan.test / password
officer@perpustakaan.test / password
student@perpustakaan.test / password
```

Password tersebut hanya untuk development lokal.

## Skenario pengujian

- User aktif dapat login.
- Password salah menampilkan pesan error.
- User tidak aktif tidak dapat login.
- Login berhasil melakukan session regeneration.
- User yang login dapat membuka `/dashboard`.
- User yang belum login diarahkan ke `/login`.
- Logout menghapus session.

## Status

- [ ] File authentication dibuat.
- [ ] Login berhasil.
- [ ] Logout berhasil.
- [ ] Route `/dashboard` terlindungi.
- [ ] Feature test authentication ditambahkan.
