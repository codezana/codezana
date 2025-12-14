# 🚀 NestJS Microservices + Docker + NATS

> **Personal step-by-step notes** to remember the *correct* process for setting up a microservices backend using **NestJS + NATS + Docker**.
>
> This is written to be saved in GitHub and reused for future projects.

---

## 🧠 Big Picture Architecture

```
Frontend
   ↓ HTTP
API Gateway (NestJS)
   ↓ NATS
Users Service (NestJS)
   ↓
Database (later: PostgreSQL / Prisma)
```

* Frontend talks **ONLY** to API Gateway
* API Gateway forwards requests via **NATS**
* Microservices never expose HTTP directly

---

## 1️⃣ Create First Project — API Gateway

```bash
nest new http-api-getway
cd http-api-getway
```

### Install required packages

```bash
npm install @nestjs/microservices nats
```

### Purpose of API Gateway

* Receives HTTP requests
* Sends messages to microservices using NATS
* No database
* No business logic

---

## 2️⃣ Configure NATS in API Gateway

Create a NATS client (example idea):

* Use `ClientsModule`
* Transport: `Transport.NATS`
* Server: `nats://nats:4222` (Docker)

API Gateway uses **request/reply** with microservices.

---

## 3️⃣ Create Second Project — Users Microservice

Go back to root folder:

```bash
cd ..
nest new users-microservices
cd users-microservices
```

### Install required packages

```bash
npm install @nestjs/microservices nats
```

### Purpose of Users Service

* Owns users business logic
* Owns users database (later)
* Listens to NATS messages
* No HTTP controllers

---

## 4️⃣ NATS Communication Rule

* API Gateway → `client.send()`
* Users Service → `@MessagePattern()`

> **Never call services directly**
> **Never share databases**

---

## 5️⃣ Docker (Development Mode)

### Important rules

* Development mode uses **watch**
* Dev mode needs **devDependencies**
* Do NOT use production Dockerfile for dev

---

## 6️⃣ Dockerfile (DEV — for ALL services)

```Dockerfile
FROM node:22.13.1

WORKDIR /app

COPY package*.json ./
COPY package-lock.json ./

RUN npm install

COPY . .

CMD ["npm", "run", "start:dev"]
```

---

## 7️⃣ docker-compose.yml (Basic)

```yaml
services:
  api-gateway:
    build: ./http-api-getway
    ports:
      - "3000:3000"
    volumes:
      - ./http-api-getway:/app
      - /app/node_modules
    command: npm run start:dev
    depends_on:
      - users-service
      - nats

  users-service:
    build: ./users-microservices
    volumes:
      - ./users-microservices:/app
      - /app/node_modules
    command: npm run start:dev
    depends_on:
      - nats

  nats:
    image: nats
    ports:
      - "4222:4222"
```

---

## 8️⃣ Important: How to Run Docker

### Start containers

```bash
docker compose up --build
```

### Stop containers

```bash
docker compose down
```

---

## 9️⃣ Docker Error Fix (VERY IMPORTANT)

If you see errors like:

* `snapshot does not exist`
* `failed to prepare extraction snapshot`

### ✅ SAFE FIX (CACHE CLEAN)

```bash
docker compose down
```

```bash
docker builder prune -af
```

```bash
docker image prune -af
```

```bash
docker volume prune -f   # optional but recommended
```

Then restart Docker Desktop and run:

```bash
docker compose up -d
```

---

## 🔟 Key Rules to Remember

* API Gateway = HTTP only
* Microservices = message-based only
* `/app` is Docker folder, NOT `src`
* Volumes override Dockerfile files
* Use `npx nest` or `npm run start:dev`
* Never rely on global tools inside Docker

---
---

# ❤️ Author

**Zana Hidayat Mohammed**  
Suli Core — Modern Full-stack Development  

