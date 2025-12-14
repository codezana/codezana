# 🗂️ Backend Folder Structure (Monorepo — Recommended)

This document explains a **clean, production-ready backend monorepo structure** for **NestJS microservices** using **API Gateway + NATS + Prisma + Docker**.

This is not beginner theory — it reflects **real-world structure** and **clear responsibilities**.

---

## 🧠 Why Monorepo?

A **monorepo** means:

* One Git repository
* Multiple backend applications
* Shared libraries
* One Docker & infra setup

### Benefits

* Easier refactoring
* Shared DTOs & events
* One deployment configuration
* Clear service boundaries

---

## 📁 Root Structure

```
backend/
│
├── docker-compose.yml      # Run all services together
├── .env                    # Shared environment variables
├── README.md               # Backend documentation
│
├── apps/                   # All runnable backend apps
├── libs/                   # Shared code (NO business logic)
├── infra/                  # Infrastructure config (Docker, NATS, DB)
```

---

## 📦 apps/ (Runnable Applications)

Each folder inside `apps/` is a **standalone NestJS application**.

---

## 🌐 apps/api-gateway/

**Purpose:**

* Entry point for frontend
* HTTP only
* Forwards requests via NATS
* No database

```
apps/api-gateway/
├── Dockerfile
├── package.json
├── tsconfig.json
└── src/
    ├── main.ts
    ├── app.module.ts
    │
    ├── health/              # Health check endpoints
    │   └── health.controller.ts
    │
    ├── users/               # HTTP → Users Service
    │   ├── users.controller.ts
    │   └── users.service.ts
    │
    ├── posts/               # HTTP → Posts Service
    │   ├── posts.controller.ts
    │   └── posts.service.ts
    │
    └── nats/
        └── nats.client.ts   # NATS request/reply client
```

### Key Rules (API Gateway)

* ✅ Uses `@Controller()` (HTTP)
* ❌ No Prisma
* ❌ No repositories
* ❌ No business logic

---

## 👤 apps/users-service/

**Purpose:**

* Users domain
* Authentication
* Owns users database
* Listens to NATS messages

```
apps/users-service/
├── Dockerfile
├── package.json
├── prisma/
│   ├── schema.prisma
│   └── migrations/
└── src/
    ├── main.ts
    ├── app.module.ts
    │
    ├── users/
    │   ├── users.controller.ts     # @MessagePattern
    │   ├── users.service.ts        # Business logic
    │   ├── users.repository.ts     # DB access
    │   ├── dto/
    │   └── events/
    │       └── user-created.event.ts
    │
    ├── auth/                       # Auth logic (JWT, hashing)
    │
    ├── prisma/
    │   └── prisma.service.ts
    │
    └── nats/
        └── nats.listener.ts        # Message subscriptions
```

### Key Rules (Users Service)

* ❌ No HTTP controllers
* ✅ Owns its database
* ✅ Emits events (user.created)
* ✅ Single responsibility

---

## 📝 apps/posts-service/

**Purpose:**

* Posts domain
* Owns posts database
* Reacts to user events

```
apps/posts-service/
├── Dockerfile
├── package.json
├── prisma/
│   ├── schema.prisma
│   └── migrations/
└── src/
    ├── main.ts
    ├── app.module.ts
    │
    ├── posts/
    │   ├── posts.controller.ts     # @MessagePattern
    │   ├── posts.service.ts
    │   ├── posts.repository.ts
    │   ├── dto/
    │   └── listeners/
    │       └── user-created.listener.ts
    │
    ├── prisma/
    │   └── prisma.service.ts
    │
    └── nats/
        └── nats.listener.ts
```

### Key Rules (Posts Service)

* ❌ No HTTP
* ❌ No direct Users DB access
* ✅ Reacts to events from Users Service

---

## 📚 libs/ (Shared Warehouse)

**Purpose:**

* Prevent duplication
* Shared contracts only

```
libs/
├── common/
│   ├── dto/           # Shared DTOs
│   ├── constants/     # Event names, roles
│   ├── utils/         # Helpers
│   └── interfaces/    # Shared interfaces
│
└── messaging/
    ├── nats.module.ts # Shared NATS module
    └── patterns.ts    # Message patterns
```

### Rules for libs/

* ❌ No Prisma
* ❌ No Nest services with logic
* ✅ Pure reusable code

---

## 🏗️ infra/ (Infrastructure)

**Purpose:**

* How the system runs
* Not application code

```
infra/
├── nats/
│   └── nats.conf      # NATS config (JetStream, auth)
│
└── postgres/
    └── init.sql       # DB init scripts
```

---

## 🧠 Final Mental Model (IMPORTANT)

* `apps/` → What runs
* `libs/` → What is shared
* `infra/` → How things run

> **Same filenames across services do NOT mean same responsibility.**

---

---

# ❤️ Author

**Zana Hidayat Mohammed**  
Suli Core — Modern Full-stack Development  
