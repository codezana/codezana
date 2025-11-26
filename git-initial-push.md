# 🚀 Git Initialization & First Push Guide

A complete step-by-step guide for connecting a new project to GitHub and pushing it for the first time.

---

## 1️⃣ Initialize Git inside your project

```bash
git init
```

---

## 2️⃣ Add all project files

```bash
git add .
```

---

## 3️⃣ Create your first commit

```bash
git commit -m "Initial project commit"
```

---

## 4️⃣ *Optional but recommended:* Set your Git username and email

```bash
git config --global user.name "codezana"
git config --global user.email "your_email_connected_to_github@gmail.com"
```

---

## 5️⃣ Connect your local project to your GitHub repository

```bash
git remote add origin https://github.com/codezana/warehouse-harem-hospital.git
```

---

## 6️⃣ Create `main` branch (if not created yet)

```bash
git branch -M main
```

---

## 7️⃣ If the repo already has README, LICENSE, or other files → pull them first

```bash
git pull origin main --allow-unrelated-histories
```

---

## 8️⃣ Fix merge conflicts if any, then commit

```bash
git add .
git commit -m "Merged remote and local changes"
```

---

## 9️⃣ Push your project to GitHub

```bash
git push -u origin main
```

---

### ✅ Your project is now fully connected to GitHub.

