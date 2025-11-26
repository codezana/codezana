# 🚀 Deployment Command List (Laravel)

A complete and clean list of commands used during Laravel project deployment on a production server.

---

## 📌 Pull Latest Code

```bash
git pull origin main
```

---

## 📌 Install Dependencies (Production Mode)

```bash
composer install --prefer-dist --no-dev --optimize-autoloader
```

---

## 📌 Environment Setup

```bash
cp .env.example .env
php artisan key:generate
```

---

## 📌 Run Migrations (Force for Production)

```bash
php artisan migrate --force
```

---

## 📌 Storage Link

```bash
php artisan storage:link
```

---

## 📌 Cache Everything

```bash
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

---

## 📌 Restart Queue Workers (If using Supervisor)

```bash
sudo supervisorctl restart laravel-queue:*
```

---

### ✅ This file is useful when deploying new Laravel projects or updating existing ones.

