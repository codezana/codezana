# 📦 Cashier Project Setup

This project is built using **Laravel + Filament v3 + SQLite + Spatie Permission + Filament Shield**.

---

## 🚀 Create Laravel Project

```bash
composer create-project laravel/laravel cashier
cd cashier
```

---

## 🎛 Install Filament v3

```bash
composer require filament/filament
php artisan filament:install --panels
```

---

## 🗄 Configure SQLite (Offline Development)

Create the SQLite database file:

```bash
touch database/database.sqlite
```

Or manually create:

```
database/database.sqlite
```

Update your `.env` file:

```env
DB_CONNECTION=sqlite
DB_DATABASE=database/database.sqlite
```

---

## 🔐 Install Spatie Roles & Permissions

```bash
composer require spatie/laravel-permission:^6.24 -W
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
php artisan migrate
```

---

## 🛡 Install Filament Shield (Role Management UI)

```bash
composer require bezhansalleh/filament-shield:^4.1 -W
php artisan shield:setup

```

---

## ✅ Tech Stack

- Laravel
- Filament v5
- SQLite (Offline Mode)
- Spatie Laravel Permission
- Filament Shield
- super admin : php artisan shield:super-admin

---

## 🧹 Optional: Clear Cache

```bash
php artisan optimize:clear
```

---

### 📌 Notes

- Make sure `database/database.sqlite` exists before running migrations.
- Recommended for offline/local development.
