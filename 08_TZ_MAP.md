<![CDATA[<div align="center">

# 🗺 TZ XARITASI (TZ MAP)

### NafGroup CRM — Texnik Hujjatlar Navigatsiyasi

---

*Barcha hujjatlar orasidagi aloqalar va tuzilma*

</div>

<br/>

---

## 🌐 1. UMUMIY XARITA

```mermaid
graph TB
    subgraph MAP["🗺 NAFGROUP CRM — TZ XARITASI"]
        direction TB
        
        RESULT["📊 00 — NATIJALAR<br/>Taqqoslash · Afzalliklar"]
        
        TZ["📋 01 — TEXNIK TOPSHIRIQ<br/>Asosiy TZ · 8 Modul · RBAC · Gantt"]
        
        ARCH["🏗 02 — ARXITEKTURA<br/>Tizim tuzilishi · Papkalar · Docker"]
        
        DB["🗄 03 — DATABASE<br/>ER Diagramma · SQL DDL · Triggerlar"]
        
        API["🔌 04 — API SPEC<br/>60+ Endpoint · JSON · Rollar"]
        
        WF["🔄 05 — WORKFLOW<br/>7 Jarayon · Status Flow · Sequence"]
        
        UI["🎨 06 — UI/UX<br/>Sitemap · Dizayn · Responsive"]
        
        SEC["🔐 07 — SECURITY + DEPLOY<br/>JWT · Docker · CI/CD · Backup"]
    end

    RESULT -.->|"taqqoslash"| TZ
    TZ -->|"arxitektura"| ARCH
    TZ -->|"database"| DB
    TZ -->|"api"| API
    TZ -->|"jarayonlar"| WF
    TZ -->|"dizayn"| UI
    TZ -->|"deploy"| SEC

    ARCH -->|"papka → kod"| DB
    ARCH -->|"papka → kod"| API
    DB -->|"jadval → endpoint"| API
    WF -->|"jarayon → sahifa"| UI
    API -->|"endpoint → sahifa"| UI
    DB -->|"migration → deploy"| SEC

    style MAP fill:#F8FAFC
    style TZ fill:#1565C0,color:#fff
    style ARCH fill:#E65100,color:#fff
    style DB fill:#2E7D32,color:#fff
    style API fill:#7B1FA2,color:#fff
    style WF fill:#00838F,color:#fff
    style UI fill:#AD1457,color:#fff
    style SEC fill:#37474F,color:#fff
    style RESULT fill:#F57F17,color:#fff
```

---

<br/>

## 📂 2. HUJJATLAR TUZILISHI

```
📁 NafGroupCrm/docs/
│
├── 📊 00_NATIJALAR_VA_TAQQOSLASH.md    ← v1.0 vs v2.0 taqqoslash
│
├── 📋 01_TEXNIK_TOPSHIRIQ.md           ← ASOSIY HUJJAT (boshlanish nuqtasi)
│   │
│   ├──→ 🏗 02_ARXITEKTURA.md           ← Tizim qanday qurilgan?
│   │       ├── Backend papka tuzilishi
│   │       ├── Frontend papka tuzilishi
│   │       └── Docker servislar
│   │
│   ├──→ 🗄 03_DATABASE_SCHEMA.md       ← Qanday jadvallar bor?
│   │       ├── ER diagramma (vizual)
│   │       ├── SQL DDL (kod)
│   │       └── Triggerlar + Indekslar
│   │
│   ├──→ 🔌 04_API_SPECIFICATION.md     ← Qanday API endpoint'lar?
│   │       ├── Auth, Orders, Clients...
│   │       ├── Request/Response JSON
│   │       └── Rol-based access
│   │
│   ├──→ 🔄 05_WORKFLOW_DIAGRAMS.md     ← Jarayonlar qanday ishlaydi?
│   │       ├── Buyurtma hayot sikli
│   │       ├── Sklad kirim-chiqim
│   │       ├── HR davomat
│   │       ├── Ish haqi hisoblash
│   │       ├── Xizmat zakazi
│   │       ├── To'lov boshqaruvi
│   │       └── Login navigatsiya
│   │
│   ├──→ 🎨 06_UI_UX_DESIGN.md          ← Qanday ko'rinishi kerak?
│   │       ├── Sahifalar xaritasi
│   │       ├── Layout wireframe
│   │       ├── Ranglar + Tipografiya
│   │       ├── Komponentlar
│   │       └── Animatsiyalar
│   │
│   └──→ 🔐 07_SECURITY_DEPLOY.md       ← Qanday deploy qilinadi?
│           ├── Xavfsizlik choralari
│           ├── Docker Compose
│           ├── Nginx konfiguratsiya
│           ├── CI/CD pipeline
│           ├── Backup strategiya
│           └── Environment variables
│
└── 🗺 08_TZ_MAP.md                     ← SIZ HOZIR SHU YERDASZ
```

---

<br/>

## 🔗 3. MODULLAR → HUJJATLAR BOG'LIQLIGI

> Har bir CRM moduli qaysi hujjatlarda tasvirlangan?

```mermaid
graph LR
    subgraph Modules["📦 CRM MODULLAR"]
        M1["📋 Buyurtmalar"]
        M2["👥 Mijozlar"]
        M3["🏗 Sklad"]
        M4["👷 HR + Davomat"]
        M5["🔧 Xizmatlar"]
        M6["💰 Moliya"]
        M7["📊 Dashboard"]
        M8["🤖 Telegram Bot"]
    end

    subgraph Docs["📄 HUJJATLAR"]
        D1["📋 01_TZ"]
        D2["🏗 02_ARCH"]
        D3["🗄 03_DB"]
        D4["🔌 04_API"]
        D5["🔄 05_WF"]
        D6["🎨 06_UI"]
        D7["🔐 07_SEC"]
    end

    M1 --> D1 & D3 & D4 & D5 & D6
    M2 --> D1 & D3 & D4 & D6
    M3 --> D1 & D3 & D4 & D5 & D6
    M4 --> D1 & D3 & D4 & D5 & D6
    M5 --> D1 & D3 & D4 & D5 & D6
    M6 --> D1 & D3 & D4 & D5 & D6
    M7 --> D1 & D4 & D6
    M8 --> D1 & D2 & D4 & D5

    style Modules fill:#E3F2FD
    style Docs fill:#FFF3E0
```

| Modul | 01 TZ | 02 Arch | 03 DB | 04 API | 05 WF | 06 UI | 07 Sec |
|:------|:-----:|:-------:|:-----:|:------:|:-----:|:-----:|:------:|
| 📋 **Buyurtmalar** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 👥 **Mijozlar** | ✅ | — | ✅ | ✅ | — | ✅ | — |
| 🏗 **Sklad** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 👷 **HR** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 🔧 **Xizmatlar** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 💰 **Moliya** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 📊 **Dashboard** | ✅ | — | — | ✅ | — | ✅ | — |
| 🤖 **Telegram Bot** | ✅ | ✅ | — | ✅ | ✅ | — | — |
| 🔑 **Auth / RBAC** | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| 🐳 **Deploy** | — | ✅ | — | — | — | — | ✅ |

---

<br/>

## 👤 4. ROLLAR → HUJJATLAR

> Har bir jamoa a'zosi qaysi hujjatlarni o'qishi kerak?

```mermaid
graph TB
    subgraph Team["👥 JAMOA"]
        PM["📊 Loyiha Rahbari"]
        BE["👨‍💻 Backend Dev"]
        FE["🌐 Frontend Dev"]
        DS["🎨 Dizayner"]
        DO["🔧 DevOps"]
        QA["🧪 QA Tester"]
    end

    subgraph Required["📄 MAJBURIY O'QISH"]
        D01["📋 01_TZ"]
        D02["🏗 02_ARCH"]
        D03["🗄 03_DB"]
        D04["🔌 04_API"]
        D05["🔄 05_WF"]
        D06["🎨 06_UI"]
        D07["🔐 07_SEC"]
    end

    PM --> D01 & D05
    BE --> D02 & D03 & D04
    FE --> D02 & D04 & D06
    DS --> D01 & D06
    DO --> D02 & D07
    QA --> D01 & D04 & D05

    style Team fill:#E3F2FD
    style Required fill:#FFF3E0
```

| Rol | Asosiy hujjatlar | Qo'shimcha |
|:----|:-----------------|:-----------|
| 📊 **Loyiha rahbari** | `01_TZ` · `05_WORKFLOW` | `00_NATIJALAR` |
| 👨‍💻 **Backend dev** | `02_ARCH` · `03_DB` · `04_API` | `05_WORKFLOW` |
| 🌐 **Frontend dev** | `02_ARCH` · `04_API` · `06_UI` | `05_WORKFLOW` |
| 🎨 **Dizayner** | `01_TZ` · `06_UI` | `05_WORKFLOW` |
| 🔧 **DevOps** | `02_ARCH` · `07_SECURITY` | — |
| 🧪 **QA Tester** | `01_TZ` · `04_API` · `05_WORKFLOW` | `06_UI` |

---

<br/>

## 📅 5. ISHLAB CHIQISH BOSQICHLARI → HUJJATLAR

> Qaysi bosqichda qaysi hujjatga murojaat qilinadi?

```mermaid
gantt
    title 📅 Bosqich → Hujjat xaritasi
    dateFormat YYYY-MM-DD
    axisFormat %d %b

    section 📐 1. ASOS
    02_ARCH + 03_DB + 07_SEC       :a1, 2025-04-14, 13d

    section 📋 2. BUYURTMALAR
    01_TZ + 03_DB + 04_API + 05_WF :b1, 2025-04-28, 15d

    section 👥 3. MIJOZ + SKLAD
    01_TZ + 03_DB + 04_API         :c1, 2025-05-13, 10d

    section 👷 4. HR + BOT
    01_TZ + 03_DB + 04_API + 05_WF :d1, 2025-05-23, 14d

    section 🔧 5. XIZMATLAR
    01_TZ + 04_API + 05_WF         :e1, 2025-06-06, 8d

    section 💰 6. MOLIYA + DASHBOARD
    01_TZ + 04_API + 06_UI         :f1, 2025-06-14, 13d

    section ✅ 7. TEST + DEPLOY
    04_API + 06_UI + 07_SEC        :g1, 2025-06-27, 10d
```

| # | Bosqich | Kerakli hujjatlar |
|:-:|:--------|:------------------|
| 1 | 📐 **Asos** (Docker, DB, Auth) | `02_ARCH` → `03_DB` → `07_SEC` |
| 2 | 📋 **Buyurtmalar** | `01_TZ §4` → `03_DB (orders)` → `04_API §2` → `05_WF §1` |
| 3 | 👥 **Mijoz + Sklad** | `01_TZ §6-7` → `03_DB (clients, products)` → `04_API §3-4` |
| 4 | 👷 **HR + Bot** | `01_TZ §5` → `03_DB (workers)` → `04_API §5` → `05_WF §3-4` |
| 5 | 🔧 **Xizmatlar** | `01_TZ §8` → `04_API §6` → `05_WF §5` |
| 6 | 💰 **Moliya + Dashboard** | `01_TZ §9-10` → `04_API §7-8` → `06_UI §4` |
| 7 | ✅ **Test + Deploy** | `04_API` → `06_UI` → `07_SEC` |

---

<br/>

## 📊 6. HUJJATLAR ORASIDAGI MA'LUMOT OQIMI

```mermaid
flowchart TD
    subgraph Layer1["📋 1-QATLAM: NIMA QILAMIZ?"]
        TZ["📋 01_TEXNIK_TOPSHIRIQ<br/>━━━━━━━━━━━━━━━━━━<br/>8 ta modul tavsifi<br/>Biznes talablar<br/>RBAC matritsasi<br/>Ishlab chiqish rejasi"]
    end

    subgraph Layer2["🏗 2-QATLAM: QANDAY QURAMIZ?"]
        ARCH["🏗 02_ARXITEKTURA<br/>━━━━━━━━━━━━━━━━━━<br/>Tizim arxitekturasi<br/>Papka tuzilishi<br/>Docker servislar"]
        WF["🔄 05_WORKFLOW<br/>━━━━━━━━━━━━━━━━━━<br/>7 ta jarayon oqimi<br/>Status flow<br/>Edge case'lar"]
    end

    subgraph Layer3["🔧 3-QATLAM: NIMADAN FOYDALANMIZ?"]
        DB["🗄 03_DATABASE<br/>━━━━━━━━━━━━━━━━━━<br/>23 ta jadval<br/>SQL DDL<br/>Triggerlar"]
        API["🔌 04_API_SPEC<br/>━━━━━━━━━━━━━━━━━━<br/>60+ endpoint<br/>JSON formatlar<br/>Rol-based access"]
        UI["🎨 06_UI/UX<br/>━━━━━━━━━━━━━━━━━━<br/>Sahifalar xaritasi<br/>Dizayn tizimi<br/>Komponentlar"]
    end

    subgraph Layer4["🚀 4-QATLAM: QANDAY ISHGA TUSHIRAMIZ?"]
        SEC["🔐 07_SECURITY_DEPLOY<br/>━━━━━━━━━━━━━━━━━━<br/>Xavfsizlik · Docker<br/>Nginx · CI/CD<br/>Backup · Monitoring"]
    end

    TZ ==>|"talablar"| ARCH
    TZ ==>|"jarayonlar"| WF
    ARCH ==>|"tuzilma"| DB
    ARCH ==>|"tuzilma"| API
    WF ==>|"foydalanuvchi tajribasi"| UI
    DB ==>|"jadvallar → endpointlar"| API
    API ==>|"ma'lumotlar → sahifalar"| UI
    DB & API & UI ==>|"barcha kodlar"| SEC

    style Layer1 fill:#E3F2FD,stroke:#1565C0
    style Layer2 fill:#FFF3E0,stroke:#E65100
    style Layer3 fill:#E8F5E9,stroke:#2E7D32
    style Layer4 fill:#F3E5F5,stroke:#7B1FA2
```

---

<div align="center">

### 🧭 QANDAY BOSHLASH KERAK?

```
1️⃣  01_TZ ni o'qing          → Tizim nima qilishini tushuning
2️⃣  02_ARCH ni o'qing         → Qanday qurilganini tushuning
3️⃣  O'z rolingizga mos        → hujjatni oching va ishlang!
```

---

*🗺 TZ Map yakunlandi*

`NafGroup CRM` · `TZ v2.0` · `2025`

</div>
]]>
