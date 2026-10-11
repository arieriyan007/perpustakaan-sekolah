# 6. Panduan Coding Manual

Masukkan setiap contoh kode secara manual ke file yang disebutkan. Setelah satu bagian selesai, jalankan test sebelum lanjut.

## 6.1 Generate file

```bash
php artisan make:model Category -m
php artisan make:model Member -m
php artisan make:model Loan -m
php artisan make:model LoanItem -m
php artisan make:controller BookController --resource
php artisan make:request StoreBookRequest
php artisan make:test LoanTest
```

## 6.2 Model `Book`

Posisi file: `app/Models/Book.php`

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Factories\HasFactory;
use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\SoftDeletes;

class Book extends Model
{
    use HasFactory, SoftDeletes;

    protected $fillable = [
        'category_id', 'code', 'isbn', 'title', 'author',
        'publisher', 'published_year', 'stock_total',
        'stock_available', 'description',
    ];

    protected function casts(): array
    {
        return [
            'published_year' => 'integer',
            'stock_total' => 'integer',
            'stock_available' => 'integer',
        ];
    }

    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    public function loanItems()
    {
        return $this->hasMany(LoanItem::class);
    }
}
```

## 6.3 Migration buku

Posisi file: `database/migrations/YYYY_MM_DD_HHMMSS_create_books_table.php`

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('books', function (Blueprint $table) {
            $table->id();
            $table->foreignId('category_id')->constrained()->restrictOnDelete();
            $table->string('code', 50)->unique();
            $table->string('isbn', 30)->nullable();
            $table->string('title');
            $table->string('author', 150);
            $table->string('publisher', 150)->nullable();
            $table->unsignedSmallInteger('published_year')->nullable();
            $table->unsignedInteger('stock_total')->default(0);
            $table->unsignedInteger('stock_available')->default(0);
            $table->text('description')->nullable();
            $table->timestamps();
            $table->softDeletes();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('books');
    }
};
```

## 6.4 Form Request buku

Posisi file: `app/Http/Requests/StoreBookRequest.php`

```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StoreBookRequest extends FormRequest
{
    public function authorize(): bool
    {
        return in_array($this->user()?->role, ['admin', 'officer'], true);
    }

    public function rules(): array
    {
        return [
            'category_id' => ['required', 'exists:categories,id'],
            'code' => ['required', 'string', 'max:50', 'unique:books,code'],
            'title' => ['required', 'string', 'max:255'],
            'author' => ['required', 'string', 'max:150'],
            'stock_total' => ['required', 'integer', 'min:0'],
            'description' => ['nullable', 'string'],
        ];
    }
}
```

Untuk update, buat `UpdateBookRequest` dan ubah rule unique agar mengabaikan ID buku yang diedit.

## 6.5 Controller buku

Posisi file: `app/Http/Controllers/BookController.php`

```php
<?php

namespace App\Http\Controllers;

use App\Http\Requests\StoreBookRequest;
use App\Models\Book;
use Illuminate\Http\RedirectResponse;
use Illuminate\View\View;

class BookController extends Controller
{
    public function index(): View
    {
        $books = Book::query()
            ->with('category')
            ->latest()
            ->paginate(15);

        return view('books.index', compact('books'));
    }

    public function store(StoreBookRequest $request): RedirectResponse
    {
        $data = $request->validated();
        $data['stock_available'] = $data['stock_total'];

        Book::create($data);

        return to_route('books.index')
            ->with('success', 'Buku berhasil ditambahkan.');
    }
}
```

Controller hanya mengatur alur request. Pindahkan proses yang kompleks ke service.

## 6.6 Route buku

Posisi file: `routes/web.php`

```php
<?php

use App\Http\Controllers\BookController;
use Illuminate\Support\Facades\Route;

Route::middleware('auth')->group(function () {
    Route::resource('books', BookController::class);
});
```

Middleware `auth` hanya memastikan login. Hak akses tetap harus divalidasi lewat Form Request, middleware, atau policy.

## 6.7 Blade daftar buku

Posisi file: `resources/views/books/index.blade.php`

```blade
@extends('layouts.app')

@section('content')
    <h1>Daftar Buku</h1>

    @if (session('success'))
        <div role="alert">{{ session('success') }}</div>
    @endif

    <a href="{{ route('books.create') }}">Tambah Buku</a>

    <table>
        <thead>
            <tr>
                <th>Kode</th>
                <th>Judul</th>
                <th>Kategori</th>
                <th>Stok</th>
            </tr>
        </thead>
        <tbody>
            @forelse ($books as $book)
                <tr>
                    <td>{{ $book->code }}</td>
                    <td>{{ $book->title }}</td>
                    <td>{{ $book->category->name }}</td>
                    <td>{{ $book->stock_available }}</td>
                </tr>
            @empty
                <tr><td colspan="4">Belum ada data buku.</td></tr>
            @endforelse
        </tbody>
    </table>

    {{ $books->links() }}
@endsection
```

Gunakan `{{ }}` untuk output yang di-escape. Hindari `{!! !!}` kecuali HTML sudah dipercaya dan disanitasi.

## 6.8 Service peminjaman

Posisi file: `app/Services/LoanService.php`

```php
<?php

namespace App\Services;

use App\Models\Book;
use App\Models\Loan;
use Illuminate\Support\Facades\DB;
use RuntimeException;

class LoanService
{
    public function create(array $data): Loan
    {
        return DB::transaction(function () use ($data) {
            $loan = Loan::create([
                'member_id' => $data['member_id'],
                'user_id' => $data['user_id'],
                'loan_number' => $data['loan_number'],
                'loan_date' => $data['loan_date'],
                'due_date' => $data['due_date'],
                'status' => 'active',
            ]);

            foreach ($data['book_ids'] as $bookId) {
                $book = Book::query()->lockForUpdate()->findOrFail($bookId);

                if ($book->stock_available < 1) {
                    throw new RuntimeException("Buku {$book->title} sedang tidak tersedia.");
                }

                $loan->items()->create([
                    'book_id' => $book->id,
                    'status' => 'borrowed',
                    'fine_amount' => 0,
                ]);

                $book->decrement('stock_available');
            }

            return $loan->load('items.book');
        });
    }
}
```

Tambahkan validasi jumlah buku, status anggota, dan pembuatan nomor transaksi sebelum service dipanggil.

## 6.9 Perintah testing

```bash
php artisan migrate:fresh --seed
php artisan test
```

`migrate:fresh` menghapus seluruh tabel pada database yang dipilih. Gunakan hanya pada database development atau testing.
