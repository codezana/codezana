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
.env
```

---

Also need :

```
npm install pg dotenv

```

---
## 📌 3. Configure PostgreSQL Connection

Edit `.env`:

```
DATABASE_URL="postgresql://postgres:SuliCore2025DB@localhost:5432/hello-prisma?schema=public"
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

## 📌 5. Install Prisma PostgreSQL Adapter

Prisma 7 requires adapters:

```bash
npm install @prisma/adapter-pg
```

---

## 📌 6. Run Migration

```bash
npx prisma migrate dev --name init
```

This creates your SQL tables in PostgreSQL.

---

## 📌 7. Generate Prisma Client

```bash
npx prisma generate
```

Client will be generated in:

```
src/generated/prisma/
```

---

## 📌 8. Create PrismaService (Important)

Create file: `src/prisma/prisma.service.ts`

```ts
import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
import { ConfigService } from '@nestjs/config'; // Import ConfigService
import { PrismaPg } from '@prisma/adapter-pg';
import { PrismaClient } from 'generated/prisma/client';

@Injectable()
export class PrismaService extends PrismaClient implements OnModuleInit, OnModuleDestroy {
  private readonly databaseUrl: string;

  // Inject ConfigService here
  constructor(private readonly configService: ConfigService) {
    const url = configService.get<string>('DATABASE_URL');

    if (!url) {
        throw new Error('DATABASE_URL environment variable is not set.');
    }

    // Use the ConfigService value for the adapter
    const adapter = new PrismaPg({ connectionString: url });
    super({ adapter });
    this.databaseUrl = url;
  }

  async onModuleInit() {
    await this.$connect();
    console.log(`Prisma connected to: ${this.databaseUrl.split('@')[1]}`);
  }

  async onModuleDestroy() {
    await this.$disconnect();
  }
}

```

---

## 📌 9. Create PrismaModule

Create file: `src/prisma/prisma.module.ts`

```ts
import { Module } from '@nestjs/common';
import { PrismaService } from './prisma.service';
import { ConfigModule } from '@nestjs/config';

@Module({
  imports: [ConfigModule],
  providers: [PrismaService],
  exports: [PrismaService],
})
export class PrismaModule {}

```

---


## 📌 10. Ensure ConfigModule is imported in app.module.ts

```ts
import { Module } from '@nestjs/common';
import { ConfigModule } from '@nestjs/config'; // Import ConfigModule
import { PrismaService } from './prisma.service';

@Module({
  imports: [
    ConfigModule.forRoot({
      isGlobal: true, // Makes .env variables available everywhere
    }),
  ],
  providers: [PrismaService],
  exports: [PrismaService],
})
export class AppModule {}

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

## 📌 15. To see tables just like pgadmin
```bash
npx prisma studio
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
