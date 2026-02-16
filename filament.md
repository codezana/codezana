# Laravel Admin Panel Setup Guide

This document describes the complete setup process for a Laravel Admin Panel project using:

- Laravel (Latest Stable)
- Filament v5
- SQLite (Local Development)
- Spatie Laravel Permission
- Filament Shield v4

This structure is suitable for internal systems, ERP platforms, dashboards, SaaS backends, and management tools.

---

## 1. Project Initialization

### Create Laravel Project

```bash
composer create-project laravel/laravel project-name
cd project-name
```
### Environment Setup

```bash
cp .env.example .env
php artisan key:generate
```

### Database Configuration (SQLite for Local Development)

```bash
touch database/database.sqlite
database/database.sqlite
```

Update your `.env` file:

```bash
DB_CONNECTION=sqlite
DB_DATABASE=database/database.sqlite
```

### Install Filament Admin Panel

Install Filament :

```bash
composer require filament/filament
```

Install Filament panel :

```bash
php artisan filament:install --panels
```

Create admin user :

```bash
php artisan make:filament-user
```

Run Migrations

```bash
php artisan migrate
```

### Install Role & Permission System

Install Spatie Permission :

```bash
composer require spatie/laravel-permission:^6.0 -W
```

Publish configuration and migrations :

```
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider"
```

Run Migrations

```bash
php artisan migrate
```

## 🔐 Install Filament Shield (Role & Permission UI)

Install package :

```bash
composer require bezhansalleh/filament-shield:^4.0 -W
```

Install Shield :

```bash
php artisan shield:install
```

Generate permissions :

```bash
php artisan shield:generate
```
## Assigning Roles Manually (Using Tinker)

In some cases, you may need to manually assign roles to a user (for example, after initial setup, migration issues, or troubleshooting permission problems).

Start Tinker :

```bash
php artisan tinker
```

```bash
$user = \App\Models\User::where('email', 'cashier1@gmail.com')->first();
Role::firstOrCreate(['name' => 'cashier']);
$user->assignRole('cashier');

```

Create Super Admin :

```bash
php artisan shield:super-admin
```
---

## 🛡 Creating Filament Resources

Generate a new resource :

```bash
php artisan make:filament-resource ResourceName
```

## 🎛 Development Commands

Start Local Server :

```bash
php artisan serve                                              
```

Access admin panel :

```bash
http://127.0.0.1:8000/admin                                        
```

Clear application cache (if needed) :

```bash
php artisan optimize:clear                                       
```
---

### For realtion if had use 

```bash
php artisan make:filament-relation-manager CategoryResource products name
```

### Custom dahsboard 

```bash
php artisan make:filament-page Dashboard
php artisan make:filament-widget DashboardStats --stats-overview
```

### For publish rsource Roles use :

```bash
 php artisan shield:publish
```

## Production Recommendations

- Use MySQL or PostgreSQL in production.
- Set `APP_ENV=production`.
- Set `APP_DEBUG=false`.
- Configure proper file permissions.
- Run `php artisan config:cache` in production.
- Use HTTPS.
- Always create a Super Admin account immediately after deployment.

---

## ✅ Tech Stack

- Laravel
- Filament v5
- SQLite (Offline Mode)
- Spatie Laravel Permission
- Filament Shield

---

## 🧹 Optional: Clear Cache

```bash
php artisan optimize:clear
```

---

### 📌 Notes

- Make sure `database/database.sqlite` exists before running migrations.
- Recommended for offline/local development.
