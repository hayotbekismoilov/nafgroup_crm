<![CDATA[<div align="center">

# 🗺 TZ XARITASI

### NafGroup CRM — Texnik Hujjatlar Navigatsiyasi

---

</div>

<br/>

## 📂 HUJJATLAR

| Fayl | Tavsif |
|:-----|:-------|
| 📊 [NATIJALAR.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/NATIJALAR.md) | v1.0 vs v2.0 taqqoslash, afzalliklar, vaqt tejash |
| 📋 [TEXNIK_TOPSHIRIQ.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/TEXNIK_TOPSHIRIQ.md) | Asosiy TZ — 8 modul, RBAC, status flow, Gantt |
| 🏗 [ARXITEKTURA.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/ARXITEKTURA.md) | Tizim arxitekturasi, papka tuzilishi, Docker |
| 🗄 [DATABASE_SCHEMA.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/DATABASE_SCHEMA.md) | ER diagramma, 23 ta jadval SQL, triggerlar |
| 🔌 [API_SPECIFICATION.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/API_SPECIFICATION.md) | 60+ REST endpoint, JSON, rol-based access |
| 🔄 [WORKFLOW_DIAGRAMS.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/WORKFLOW_DIAGRAMS.md) | 7 ta jarayon oqimi — buyurtma, HR, to'lov |
| 🎨 [UI_UX_DESIGN.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/UI_UX_DESIGN.md) | Sahifalar xaritasi, dizayn tizimi, responsive |
| 🔐 [SECURITY_DEPLOY.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/SECURITY_DEPLOY.md) | JWT, Docker Compose, Nginx, CI/CD, backup |
| 🗺 [TZ_MAP.md](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/TZ_MAP.md) | Shu hujjat — navigatsiya xaritasi |

---

<br/>

## 🌐 ALOQALAR XARITASI

```mermaid
graph TB
    subgraph MAP["🗺 NAFGROUP CRM — TZ XARITASI"]
        direction TB

        RESULT["📊 NATIJALAR<br/>Taqqoslash · Afzalliklar"]

        TZ["📋 TEXNIK TOPSHIRIQ<br/>8 Modul · RBAC · Gantt"]

        ARCH["🏗 ARXITEKTURA<br/>Tizim · Papkalar · Docker"]

        DB["🗄 DATABASE SCHEMA<br/>ER · SQL · Triggerlar"]

        API["🔌 API SPECIFICATION<br/>60+ Endpoint · JSON · Rollar"]

        WF["🔄 WORKFLOW DIAGRAMS<br/>7 Jarayon · Status Flow"]

        UI["🎨 UI/UX DESIGN<br/>Sitemap · Ranglar · Responsive"]

        SEC["🔐 SECURITY + DEPLOY<br/>JWT · Docker · CI/CD · Backup"]
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

## 👤 KIM NIMANI O'QIYDI?

| Rol | Majburiy | Qo'shimcha |
|:----|:---------|:-----------|
| 📊 **Loyiha rahbari** | [TEXNIK_TOPSHIRIQ](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/TEXNIK_TOPSHIRIQ.md) · [WORKFLOW](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/WORKFLOW_DIAGRAMS.md) | [NATIJALAR](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/NATIJALAR.md) |
| 👨‍💻 **Backend dev** | [ARXITEKTURA](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/ARXITEKTURA.md) · [DATABASE](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/DATABASE_SCHEMA.md) · [API](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/API_SPECIFICATION.md) | [WORKFLOW](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/WORKFLOW_DIAGRAMS.md) |
| 🌐 **Frontend dev** | [ARXITEKTURA](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/ARXITEKTURA.md) · [API](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/API_SPECIFICATION.md) · [UI/UX](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/UI_UX_DESIGN.md) | [WORKFLOW](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/WORKFLOW_DIAGRAMS.md) |
| 🎨 **Dizayner** | [TEXNIK_TOPSHIRIQ](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/TEXNIK_TOPSHIRIQ.md) · [UI/UX](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/UI_UX_DESIGN.md) | [WORKFLOW](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/WORKFLOW_DIAGRAMS.md) |
| 🔧 **DevOps** | [ARXITEKTURA](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/ARXITEKTURA.md) · [SECURITY](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/SECURITY_DEPLOY.md) | — |
| 🧪 **QA Tester** | [TEXNIK_TOPSHIRIQ](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/TEXNIK_TOPSHIRIQ.md) · [API](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/API_SPECIFICATION.md) · [WORKFLOW](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/WORKFLOW_DIAGRAMS.md) | [UI/UX](file:///c:/Users/ZenBook/OneDrive/Рабочий%20стол/NafGroupCrm/docs/UI_UX_DESIGN.md) |

---

<br/>

## 📦 MODUL → HUJJAT

| Modul | TZ | Arch | DB | API | WF | UI | Sec |
|:------|:--:|:----:|:--:|:---:|:--:|:--:|:---:|
| 📋 **Buyurtmalar** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 👥 **Mijozlar** | ✅ | — | ✅ | ✅ | — | ✅ | — |
| 🏗 **Sklad** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 👷 **HR + Davomat** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 🔧 **Xizmatlar** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 💰 **Moliya** | ✅ | — | ✅ | ✅ | ✅ | ✅ | — |
| 📊 **Dashboard** | ✅ | — | — | ✅ | — | ✅ | — |
| 🤖 **Telegram Bot** | ✅ | ✅ | — | ✅ | ✅ | — | — |
| 🔑 **Auth / RBAC** | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ |
| 🐳 **Deploy** | — | ✅ | — | — | — | — | ✅ |

---

<br/>

## 📊 MA'LUMOT OQIMI

```mermaid
flowchart TD
    subgraph L1["📋 NIMA QILAMIZ?"]
        TZ["TEXNIK TOPSHIRIQ<br/>Biznes talablar · Modullar"]
    end

    subgraph L2["🏗 QANDAY QURAMIZ?"]
        ARCH["ARXITEKTURA<br/>Tuzilma · Servislar"]
        WF["WORKFLOW<br/>Jarayonlar · Oqimlar"]
    end

    subgraph L3["🔧 NIMADAN FOYDALANAMIZ?"]
        DB["DATABASE<br/>Jadvallar · SQL"]
        API["API SPEC<br/>Endpointlar · JSON"]
        UI["UI/UX<br/>Sahifalar · Dizayn"]
    end

    subgraph L4["🚀 QANDAY ISHGA TUSHIRAMIZ?"]
        SEC["SECURITY + DEPLOY<br/>Docker · CI/CD · Backup"]
    end

    TZ ==> ARCH & WF
    ARCH ==> DB & API
    WF ==> UI
    DB ==> API ==> UI
    DB & API & UI ==> SEC

    style L1 fill:#E3F2FD,stroke:#1565C0
    style L2 fill:#FFF3E0,stroke:#E65100
    style L3 fill:#E8F5E9,stroke:#2E7D32
    style L4 fill:#F3E5F5,stroke:#7B1FA2
```

---

<div align="center">

### 🧭 QANDAY BOSHLASH?

`1️⃣ TEXNIK_TOPSHIRIQ → 2️⃣ ARXITEKTURA → 3️⃣ O'z rolingizga mos hujjat`

---

`NafGroup CRM` · `2025`

</div>
]]>
