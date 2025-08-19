# Infrastructure Project

This repository contains the **infrastructure setup** for running the entire system using **Docker Compose**.  
It includes the **database**, **backend services**, and **frontend applications**.

---

## 📦 Services

### 1. Database
- **Service name**: `db`
- **Image**: `postgres:15`
- **Exposed Port**: `5432`
- **Volumes**:
  - `postgres_data`: persistent database storage
  - `./postgres-init`: initialization scripts executed when the container starts
- **Notes**:  
  Initializes with `POSTGRES_USER=postgres` and `POSTGRES_PASSWORD=postgres`.

---

### 2. Backend Services

#### 🟦 top-users
- **Folder**: `../top-users`  
- **Port**: `8888`  
- **Environment Variables**:
  - `DB_URL=db`
  - `DB_PORT=5432`
  - `DB_USER=postgres`
  - `DB_PASSWORD=postgres`
  - `DB_NAME=db_users`

#### 🟩 top-finance
- **Folder**: `../top-finance`  
- **Port**: `8889`  
- **Environment Variables**:
  - `DB_URL=db`
  - `DB_PORT=5432`
  - `DB_USER=postgres`
  - `DB_PASSWORD=postgres`
  - `DB_NAME=db_finance`
  - `TOP_USERS_URL=top-users`
  - `TOP_USERS_PORT=8888`

#### 🟨 top-api-gateway
- **Folder**: `../top-api-gateway`  
- **Port**: `3000`  
- **Environment Variables**:
  - `TOP_USERS_URL=top-users`
  - `TOP_USERS_PORT=8888`
  - `TOP_FINANCE_URL=top-finance`
  - `TOP_FINANCE_PORT=8889`
  - `SECRET_KEY` → secure JWT or API key

---

### 3. Frontend Applications

#### 👤 app-users
- **Folder**: `../app-users`  
- **Port**: `5001`

#### 💰 app-finance
- **Folder**: `../app-finance`  
- **Port**: `5002`

#### 🌐 app-react
- **Folder**: `../app-react`  
- **Port**: `80`

---

## Projects folder scructure
root/
│
├── infrastructure/           # 📌 The folder containning docker-compose.yml
│   ├── docker-compose.yml
│   └── postgres-init/        # database initial SQL scripts
│       └── init.sql
├── top-users/                # 🟦 [top-users Microservice (Nest.js)](https://github.com/darlisonosorio/top-users)
│   └── ...
├── top-finance/              # 🟩 [top-finance Microsservice (Nest.js)](https://github.com/darlisonosorio/top-finance)
│   └── ...
├── top-api-gateway/          # 🚪 [API Gateway (Nest.js)](https://github.com/darlisonosorio/top-api-gateway)
│   └── ...
├── app-users/                # 🖥️ [app-users (React/Vite)](https://github.com/darlisonosorio/app-users)
│   └── ...
├── app-finance/              # 🖥️ [app-finance (React/Vite)](https://github.com/darlisonosorio/app-finance)
│   └── ...
└── app-react/                # 🖥️ [app-react Main App (React/Vite)](https://github.com/darlisonosorio/app-react)
    └── ...

## ⚙️ Prerequisites

- **Docker** installed and running
  - Windows/Mac: Docker Desktop or inside WSL
  - Linux: Docker Engine
- **Docker Compose v2**

Check install version:
```bash
docker --version
docker compose version 
```

## 🛠 How to Run

1. Navigate to the infrastructure project folder:
```bash
cd infrastructure
docker-compose up --build
```

2. Access the applications:
  - API Gateway → http://localhost:3000
  - React App → http://localhost


### 🔑 Notes
The build field in each service points to the folder name of the project, relative to this infrastructure directory.
There's no need to manually run migrations, each microservice runs the migration itself on start