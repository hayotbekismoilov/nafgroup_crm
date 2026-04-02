# 🏗 ARXITEKTURA DIAGRAMMALARI
### NafGroup CRM — Tizim Arxitekturasi

---

## 📐 1. YUQORI DARAJADAGI ARXITEKTURA

```mermaid
graph TB
    subgraph Client["🖥 KLIENT QATLAMI"]
        WEB["🌐 Web App<br/>React 18 + TS"]
        MOB["📱 PWA<br/>(2-bosqich)"]
        TG["🤖 Telegram Bot<br/>Aiogram 3"]
    end

    subgraph LB["⚖️ PROXY"]
        NGINX["📡 Nginx<br/>SSL · Reverse Proxy"]
    end

    subgraph Backend["⚙️ BACKEND"]
        DJANGO["🐍 Django 5 + DRF<br/>Gunicorn · Port 8000"]
        WS["🔌 Django Channels<br/>Daphne · WebSocket"]
        CELERY["⏰ Celery Workers<br/>Background Tasks"]
        BEAT["⏱ Celery Beat<br/>Scheduled Jobs"]
    end

    subgraph Data["💾 MA'LUMOTLAR"]
        PG["🐘 PostgreSQL 16<br/>Asosiy DB"]
        REDIS["🔴 Redis 7<br/>Cache · Broker"]
        MINIO["📦 MinIO<br/>Fayllar · S3"]
    end

    subgraph External["🌍 TASHQI"]
        TGAPI["📬 Telegram API"]
        PAY["💳 Payme · Click · Uzum"]
        SMS["📨 SMS Gateway"]
    end

    WEB & MOB -->|"HTTPS"| NGINX
    TG -->|"Webhook"| NGINX
    NGINX -->|"HTTP"| DJANGO
    NGINX -->|"WS"| WS
    DJANGO --> PG & REDIS & MINIO
    DJANGO -->|"async"| CELERY
    CELERY --> PG & REDIS
    BEAT --> REDIS
    WS --> REDIS
    CELERY --> TGAPI & SMS
    DJANGO --> PAY

    style Client fill:#E3F2FD,stroke:#1565C0
    style Backend fill:#FFF3E0,stroke:#E65100
    style Data fill:#E8F5E9,stroke:#2E7D32
    style External fill:#F3E5F5,stroke:#7B1FA2
```

---

## 📂 2. BACKEND LOYIHA TUZILISHI

```
nafgroup_crm/
│
├── 📁 config/                        ⚙️ Django konfiguratsiya
│   ├── settings/
│   │   ├── base.py                   Umumiy sozlamalar
│   │   ├── development.py            Dev muhit
│   │   └── production.py             Production muhit
│   ├── urls.py                       Asosiy URL routing
│   ├── wsgi.py                       WSGI entry point
│   ├── asgi.py                       ASGI (WebSocket)
│   └── celery.py                     Celery sozlamalari
│
├── 📁 apps/                          📦 Django ilovalari
│   │
│   ├── 📁 accounts/                  🔑 Autentifikatsiya
│   ├── 📁 orders/                    📋 Buyurtmalar
│   ├── 📁 clients/                   👥 Mijozlar
│   ├── 📁 warehouse/                 🏗 Sklad
│   ├── 📁 staff/                     👷 Ishchilar HR
│   ├── 📁 services/                  🔧 Xizmatlar
│   ├── 📁 finance/                   💰 Moliya
│   ├── 📁 dashboard/                 📊 Dashboard
│   ├── 📁 bot/                       🤖 Telegram Bot
│   └── 📁 common/                    🔧 Umumiy utils
│
├── 📁 media/                          Yuklangan fayllar (dev)
├── 📁 static/                         Statik fayllar
├── 📁 templates/                      PDF shablonlari
│
├── 📁 docker/
│   ├── Dockerfile                    Django container
│   ├── Dockerfile.bot                Telegram bot container
│   └── nginx/
│       ├── nginx.conf                Nginx konfiguratsiya
│       └── ssl/                      SSL sertifikatlar
│
├── docker-compose.yml                Dev muhit
├── docker-compose.prod.yml           Production muhit
├── 📁 requirements/
├── manage.py
├── .env.example
└── README.md
```

---

## 🌐 3. FRONTEND LOYIHA TUZILISHI

```
frontend/
│
├── 📁 src/
│   │
│   ├── 📁 app/                       ⚛️ App yadro
│   │   ├── App.tsx
│   │   └── providers/
│   │
│   ├── 📁 pages/                     📄 Sahifalar
│   │   ├── auth/
│   │   ├── dashboard/
│   │   ├── orders/
│   │   ├── clients/
│   │   ├── warehouse/
│   │   ├── staff/
│   │   ├── services/
│   │   └── finance/
│   │
│   ├── 📁 components/                🧩 Umumiy komponentlar
│   │   ├── ui/                       Shadcn/UI
│   │   └── shared/
│   │
│   ├── 📁 hooks/                     🪝 Custom hooks
│   ├── 📁 stores/                    🗄 Zustand stores
│   ├── 📁 lib/                       📚 Yordamchilar (api, utils)
│   ├── 📁 styles/
│   └── main.tsx
│
├── public/
├── index.html
├── vite.config.ts
├── tailwind.config.ts
├── tsconfig.json
└── package.json
```

---

## 🐳 4. DOCKER DEPLOY ARXITEKTURASI

```mermaid
graph TB
    subgraph Docker["🐳 Docker Compose — Production"]
        direction TB
        
        subgraph Proxy["📡 Proxy Layer"]
            NGX["📡 Nginx<br/>:80 / :443<br/>SSL + Static"]
        end

        subgraph App["⚙️ Application Layer"]
            DJ["🐍 Django<br/>Gunicorn :8000<br/>4 workers"]
            CH["🔌 Channels<br/>Daphne :8001<br/>WebSocket"]
            CL["⏰ Celery<br/>Worker ×4"]
            CB["⏱ Beat<br/>Scheduler"]
            BT["🤖 Bot<br/>Aiogram"]
        end

        subgraph Data["💾 Data Layer"]
            PG["🐘 PostgreSQL<br/>:5432"]
            RD["🔴 Redis<br/>:6379"]
            MN["📦 MinIO<br/>:9000"]
        end
    end

    NGX -->|"HTTP"| DJ
    NGX -->|"WS"| CH
    DJ --> PG & RD & MN
    CH --> RD
    CL --> PG & RD
    CB --> RD
    BT -->|"API"| DJ

    style Proxy fill:#BBDEFB,stroke:#1565C0
    style App fill:#FFE0B2,stroke:#E65100
    style Data fill:#C8E6C9,stroke:#2E7D32
```

### 4.1 Servislar ro'yxati

| # | Servis | Image | Port | Vazifa |
|:-:|:-------|:------|:----:|:-------|
| 1 | 🐘 **postgres** | `postgres:16-alpine` | 5432 | Database |
| 2 | 🔴 **redis** | `redis:7-alpine` | 6379 | Cache + Broker |
| 3 | 📦 **minio** | `minio/minio` | 9000 | File storage |
| 4 | 🐍 **django** | Custom | 8000 | REST API |
| 5 | 🔌 **channels** | Custom | 8001 | WebSocket |
| 6 | ⏰ **celery** | Custom | — | Background tasks |
| 7 | ⏱ **celery-beat** | Custom | — | Scheduled tasks |
| 8 | 🤖 **bot** | Custom | — | Telegram bot |
| 9 | 📡 **nginx** | `nginx:alpine` | 80/443 | Reverse proxy |

---

## 🔄 5. MA'LUMOTLAR OQIMI

```mermaid
flowchart TB
    subgraph Input["📥 KIRISH"]
        W["🌐 Web UI"]
        B["🤖 Bot"]
        E["🔗 External"]
    end

    subgraph Process["⚙️ QAYTA ISHLASH"]
        DRF["📡 DRF<br/>Serializers · Views"]
        SIG["⚡ Signals<br/>Auto-triggers"]
        CEL["⏰ Celery<br/>Async tasks"]
    end

    subgraph Store["💾 SAQLASH"]
        DB["🐘 PostgreSQL"]
        CA["🔴 Redis"]
        FS["📦 MinIO"]
    end

    subgraph Output["📤 CHIQISH"]
        JS["📡 JSON API"]
        WS_O["🔌 WebSocket"]
        TG_O["📬 Telegram"]
        EX["📊 Excel/PDF"]
    end

    W & B & E --> DRF
    DRF --> DB & CA & FS
    DRF --> SIG --> DB
    DRF --> CEL --> TG_O & EX
    SIG --> WS_O
    DRF --> JS

    style Input fill:#E3F2FD
    style Process fill:#FFF3E0
    style Store fill:#E8F5E9
    style Output fill:#F3E5F5
```

---

*📐 Arxitektura hujjati yakunlandi*
