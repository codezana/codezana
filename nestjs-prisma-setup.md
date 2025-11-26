# ⭐ NestJS + Prisma 7 + PostgreSQL — Complete Setup Guide

This guide shows the exact steps you need to set up Prisma 7 with NestJS and PostgreSQL in real-world projects.  
Save this file and use it for every new backend you build.

---

## 📌 1. Create a New NestJS Project

```bash
npm i -g @nestjs/cli
nest new project-name
cd project-name
```

---

## 📌 2. Install Prisma

```bash
npm install prisma --save-dev
npx prisma init
```

This creates:

```
prisma/schema.prisma
prisma/prisma.config.ts
.env
```

---

## 📌 3. Configure PostgreSQL Connection

Edit `.env`:

```
DATABASE_URL="postgresql://postgres:root@localhost:5432/mydb?schema=public"
```

Replace:

- `postgres` → your DB username  
- `root` → your DB password  
- `mydb` → your database name  

---

## 📌 4. Configure Prisma Schema (Prisma 7 Required)

Edit `prisma/schema.prisma`:

```prisma
generator client {
  provider = "prisma-client"
  output   = "../src/generated/prisma"
}

datasource db {
  provider = "postgresql"
}

model User {
  id      Int     @id @default(autoincrement())
  email   String  @unique
  name    String?
}
```

You can add more models later.

---

## 📌 5. Configure prisma.config.ts (Correct Minimal Version)

Edit `prisma/prisma.config.ts`:

```ts
import "dotenv/config";
import { defineConfig, env } from "@prisma/config";

export default defineConfig({
  schema: "./prisma/schema.prisma",
  datasource: {
    url: env("DATABASE_URL"),
  },
});
```

No extra keys needed.  
Prisma 7 reads the generator/output from `schema.prisma`.

---

## 📌 6. Install Prisma PostgreSQL Adapter

Prisma 7 requires adapters:

```bash
npm install @prisma/adapter-pg
```

---

## 📌 7. Run Migration

```bash
npx prisma migrate dev --name init
```

This creates your SQL tables in PostgreSQL.

---

## 📌 8. Generate Prisma Client

```bash
npx prisma generate
```

Client will be generated in:

```
src/generated/prisma/
```

---

## 📌 9. Create PrismaService (Important)

Create file: `src/prisma/prisma.service.ts`

```ts
import { Injectable } from '@nestjs/common';
import { PrismaClient } from '../generated/prisma/client';
import { PrismaPg } from '@prisma/adapter-pg';

@Injectable()
export class PrismaService extends PrismaClient {
  constructor() {
    const adapter = new PrismaPg({ url: process.env.DATABASE_URL });
    super({ adapter });
  }
}
```

---

## 📌 10. Create PrismaModule

Create file: `src/prisma/prisma.module.ts`

```ts
import { Module } from '@nestjs/common';
import { PrismaService } from './prisma.service';

@Module({
  providers: [PrismaService],
  exports: [PrismaService],
})
export class PrismaModule {}
```

---

## 📌 11. Import PrismaModule in Any Feature Module

Example: `users.module.ts`

```ts
@Module({
  imports: [PrismaModule], // Required
  controllers: [UsersController],
  providers: [UsersService],
})
export class UsersModule {}
```

---

## 📌 12. Use Prisma in Services

Example: `users.service.ts`

```ts
@Injectable()
export class UsersService {
  constructor(private prisma: PrismaService) {}

  findAll() {
    return this.prisma.user.findMany();
  }
}
```

---

## 📌 13. DTOs Still Required

NestJS still uses DTOs for validation:

```ts
export class CreateUserDto {
  @IsEmail()
  email: string;

  @IsOptional()
  @IsString()
  name?: string;
}
```

---

## 📌 14. Start the App

```bash
npm run start:dev
```

---

# 🎉 Your NestJS + Prisma 7 Project Is Now Ready

You can now:

- Create models  
- Generate migrations  
- Write CRUD services  
- Build real apps  

---

# ❤️ Author

**Zana Hidayat Mohammed**  
Suli Core — Modern Full-stack Development  
