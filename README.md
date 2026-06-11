# 🚀 Laravel Complete Command Reference

> A comprehensive cheat sheet for Laravel development — from setup to database seeding.

---

## 📋 Table of Contents

- [Installation & Setup](#-installation--setup)
- [Database Configuration](#-database-configuration)
- [VS Code Extensions](#-vs-code-extensions)
- [File Structure](#-file-structure)
- [Routing](#-routing)
- [Blade Templates](#-blade-templates)
- [Database Migrations](#-database-migrations)
- [Database Seeders](#-database-seeders)
- [Model Factories](#-model-factories)
- [Models & Controllers](#-models--controllers)
- [MySQL Data Types](#-mysql-data-types)

---

## ⚙️ Installation & Setup

### Step 1 — Install Composer
Download and install from [https://getcomposer.org](https://getcomposer.org)

### Step 2 — Install Laravel

```bash
# Global installer
composer global require laravel/installer

# OR via Composer (no global install needed)
composer create-project laravel/laravel example-app
```

### Step 3 — Create Project (inside XAMPP/WAMP `www` folder)

```bash
laravel new example-app
```

### ▶️ Run the Development Server

```bash
php artisan serve
```

---

## 🗄️ Database Configuration

### `.env` File Settings

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=learn_laravel
DB_USERNAME=root
DB_PASSWORD=

SESSION_DRIVER=file
CACHE_STORE=file
```

### `config/database.php` — Inside `'mysql' => []`

```php
'engine' => 'InnoDB',
```

---

## 🧩 VS Code Extensions

| Extension | Author |
|---|---|
| PHP IntelliSense | Damjan Cvetko |
| PHP Namespace Resolver | Mehedi Hassan |
| Laravel Extra Intellisense | Amir |
| laravel-blade | Christian Howe |
| Laravel Blade Snippets | Winnie Lin |
| Laravel Goto View | codingyu |

---

## 📁 File Structure

```
project/
├── app/
│   └── Http/
│       ├── Controllers/      ← Controllers
│       └── Models/           ← Models
├── resources/
│   └── views/                ← Blade Views
├── public/                   ← Assets (CSS, JS, images)
└── routes/
    └── web.php               ← Web Routes
```

---

## 🛣️ Routing

### Basic Routes — `routes/web.php`

```php
// Return a view
Route::get('/urlname', function () {
    return view('htmlfilename');
});

// Return inline HTML
Route::get('/urlname', function () {
    return "<h1>Hello</h1>";
});

// Shorthand view route
Route::view('/urlname', 'filename');

// Route with required parameter
Route::get('/user/{id}', function (string $id) {
    return 'User ' . $id;
});

// Route with optional parameter
Route::get('/user/{id?}', function (string $id = null) {
    if ($id) {
        return "<h1>Hello Roll No: $id</h1>";
    }
    return "<h1>No ID</h1>";
});

// Route with parameter constraint (numbers only)
Route::get('/user/{id?}', function (string $id) {
    if ($id) {
        return "<h1>User ID: $id</h1>";
    }
    return "<h1>No ID Found</h1>";
})->whereNumber('id');
```

### Named Routes

```php
Route::get('/urlname', function () {
    return view('htmlfilename');
})->name('as_you_wish');

// Use in Blade template:
// <a href="{{ route('as_you_wish') }}">Link</a>
```

### Redirect Routes

```php
Route::redirect('/about', '/test');         // Temporary redirect
Route::redirect('/about', '/test', 301);    // Permanent redirect
```

### Grouped Routes (with Prefix)

```php
Route::prefix('page')->group(function () {
    Route::get('/home', function () { return view('home'); });
    Route::get('/about', function () { return view('about'); });
    Route::get('/contact', function () { return view('contact'); });
});
```

### Fallback (404 Page)

```php
Route::fallback(function () {
    return "<h1>Page Not Found</h1>";
});
```

### Route Artisan Commands

```bash
php artisan route -h                      # All route commands (help)
php artisan route:list                    # List all routes
php artisan route:list --except-vendor    # Only your routes
php artisan route:list --path=tarun       # Filter by path
```

---

## 🎨 Blade Templates

### Main Directives

| Directive | Purpose |
|---|---|
| `@include('file')` | Include a sub-view |
| `@extends('layout')` | Extend a layout |
| `@section('name')` | Define a section |
| `@yield('name')` | Output a section |

---

## 🧱 Database Migrations

**Location:** `database/migrations/`

```bash
# Create a new migration file
php artisan make:migration create_students_table

# Run all pending migrations
php artisan migrate

# Rollback last batch
php artisan migrate:rollback

# Rollback a specific batch number
php artisan migrate:rollback --batch=3

# Rollback last N migrations
php artisan migrate:rollback --step=3

# Reset all migrations (drop all tables)
php artisan migrate:reset

# Fresh migration (drop + re-run all)
php artisan migrate:fresh
```

### Migration Modifiers & Column Changes

```php
// Create a separate migration for altering a table
php artisan make:migration update_students_table --table=students

// Inside the migration up() method:
$table->renameColumn('from', 'to');
$table->dropColumn('city');
$table->dropColumn(['city', 'avatar', 'location']);
$table->string('name', 50)->change();
$table->integer('votes')->unsigned()->default(1)->comment('my comment')->change();
```

### Create Model + Migration Together

```bash
php artisan make:model Student -m
```

---

## 🌱 Database Seeders

### 5-Step Process

```
1. Create Model       → php artisan make:model Student
2. Create Seeder      → php artisan make:seeder StudentSeeder
3. Write seeder logic inside StudentSeeder.php
4. Register seeder in database/seeders/DatabaseSeeder.php
5. Run the seeder
```

```bash
# Step 1 — Create model
php artisan make:model Student

# Step 2 — Create seeder
php artisan make:seeder StudentSeeder

# Step 5 — Run all seeders
php artisan db:seed

# Run a specific seeder only
php artisan db:seed --class=StudentSeeder
```

### `database/seeders/DatabaseSeeder.php`

```php
use App\Models\Student;

public function run(): void
{
    // Call your seeder here
    $this->call(StudentSeeder::class);
}
```

> 📚 **Faker Docs:** [https://fakerphp.org](https://fakerphp.org)

---

## 🏭 Model Factories

### 5-Step Process

```
1. Create Model       → php artisan make:model Student
2. Create Factory     → php artisan make:factory StudentFactory
3. Write factory definition inside StudentFactory.php
4. Register factory in database/seeders/DatabaseSeeder.php
5. Run seeder
```

```bash
# Step 1 — Create model
php artisan make:model Student

# Step 2 — Create factory
php artisan make:factory StudentFactory

# Create factory and link to model at once
php artisan make:factory StudentFactory --model=Student

# Step 5 — Run seeder
php artisan db:seed
```

### `StudentFactory.php`

```php
class StudentFactory extends Factory
{
    public function definition(): array
    {
        return [
            'name'  => fake()->name(),
            'email' => fake()->email(),
        ];
    }
}
```

### `DatabaseSeeder.php`

```php
use App\Models\Student;

Student::factory()->count(5)->create();
```

### Shorthand — Create Model + Factory Together

```bash
php artisan make:model Student -f
```

### Run Fresh Migration + Seeding Together

```bash
php artisan migrate:fresh --seed
```

---

## 🎮 Models & Controllers

```bash
# Create a Model
php artisan make:model Student

# Create a Resource Controller
php artisan make:controller StudentController --resource
```

### Register Resource Routes in `routes/web.php`

```php
use App\Http\Controllers\StudentController;

Route::resource('students', StudentController::class);
```

> This auto-generates all 7 RESTful routes: `index`, `create`, `store`, `show`, `edit`, `update`, `destroy`.

---

## 🗂️ MySQL Data Types

### String Types

| Type | Description |
|---|---|
| `CHAR(size)` | Fixed length, 0–255 chars |
| `VARCHAR(size)` | Variable length, 0–65,535 chars |
| `TINYTEXT` | Up to 255 characters |
| `TEXT(size)` | Up to 65,535 bytes |
| `MEDIUMTEXT` | Up to 16,777,215 characters |
| `LONGTEXT` | Up to 4,294,967,295 characters |

### Binary Types

| Type | Description |
|---|---|
| `TINYBLOB` | Up to 255 bytes |
| `BLOB(size)` | Up to 65,535 bytes |
| `MEDIUMBLOB` | Up to 16,777,215 bytes |
| `LONGBLOB` | Up to 4,294,967,295 bytes |

### Numeric Types

| Type | Description |
|---|---|
| `TINYINT(size)` | -128 to 127 (signed) / 0 to 255 (unsigned) |
| `SMALLINT(size)` | -32,768 to 32,767 (signed) |
| `MEDIUMINT(size)` | -8,388,608 to 8,388,607 (signed) |
| `INT(size)` | -2,147,483,648 to 2,147,483,647 (signed) |
| `BIGINT(size)` | Very large integers (±9.2 × 10¹⁸) |
| `BOOL / BOOLEAN` | 0 (false) or 1 (true) |
| `FLOAT(p)` | Approximate floating-point |
| `DOUBLE(size, d)` | Higher precision floating-point |
| `DECIMAL(size, d)` | Exact fixed-point (e.g. money) |

---

## 🔗 Useful Links

- 📦 [Composer](https://getcomposer.org)
- 🌐 [Laravel Docs](https://laravel.com/docs)
- 🎭 [Faker PHP](https://fakerphp.org)

---

<p align="center">Made with ❤️ for Laravel developers</p>
