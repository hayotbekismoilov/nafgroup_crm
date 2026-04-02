# API SPETSIFIKATSIYASI
### NafGroup CRM — REST API Endpoints

**Base URL:** `https://api.nafgroup.uz/api/v1/`  
**Auth:** `JWT Bearer Token`  
**Format:** `JSON`

---

## 🔑 1. AUTENTIFIKATSIYA

| Metod | Endpoint | Tavsif | 🔐 |
|:-----:|:---------|:-------|:--:|
| `POST` | `/auth/login/` | Tizimga kirish | ❌ |
| `POST` | `/auth/logout/` | Tizimdan chiqish | ✅ |
| `POST` | `/auth/refresh/` | Token yangilash | ❌ |
| `GET` | `/auth/me/` | Joriy foydalanuvchi | ✅ |
| `PUT` | `/auth/password/change/` | Parol o'zgartirish | ✅ |

### 📤 Login Request
```json
{
    "email": "admin@nafgroup.uz",
    "password": "securepass123"
}
```

### 📥 Login Response `200`
```json
{
    "access": "eyJ0eXAiOiJKV1Qi...",
    "refresh": "eyJ0eXAiOiJKV1Qi...",
    "user": {
        "id": "uuid",
        "full_name": "Admin",
        "role": "SUPERADMIN"
    }
}
```

---

## 📋 2. BUYURTMALAR

| Metod | Endpoint | Tavsif | Rollar |
|:-----:|:---------|:-------|:------:|
| `GET` | `/orders/` | 📋 Ro'yxat (paginated) | M O D SA |
| `POST` | `/orders/` | ➕ Yangi buyurtma | M SA |
| `GET` | `/orders/{id}/` | 📄 Batafsil | M O D SA |
| `PUT` | `/orders/{id}/` | ✏️ Tahrirlash | M SA |
| `PATCH` | `/orders/{id}/` | 🔄 Qisman tahrir | M O SA |
| `DELETE` | `/orders/{id}/` | ❌ O'chirish | SA |
| `PATCH` | `/orders/{id}/status/` | 🔄 Holat o'zgartirish | M O SA |
| `POST` | `/orders/{id}/files/` | 📎 Fayl yuklash | M SA |
| `DELETE` | `/orders/{id}/files/{fid}/` | 🗑 Fayl o'chirish | M SA |
| `GET` | `/orders/{id}/history/` | 📜 Holat tarixi | M D SA |
| `GET` | `/orders/queue/` | 📊 Navbat ro'yxati | M O SA |
| `PATCH` | `/orders/{id}/queue/` | 🔄 Navbat o'zgartirish | M SA |
| `GET` | `/orders/kanban/` | 📊 Kanban view | All |

### 🔍 Filtrlash

```text
GET /orders/?status=PRINTING
            &priority=RUSH
            &client={uuid}
            &date_from=2025-04-01
            &date_to=2025-04-30
            &search=banner
            &ordering=-deadline
            &page=1&page_size=20
```

### ➕ Buyurtma yaratish

```json
// POST /orders/
{
    "client_id": "uuid",
    "width_cm": 300.5,
    "height_cm": 200.0,
    "measurement_unit": "cm",
    "quantity": 2,
    "material_id": "uuid",
    "print_type": "ONE_SIDE",
    "deadline": "2025-04-10T18:00:00+05:00",
    "priority": "NORMAL",
    "price": 2500000,
    "notes": "Maxsus talablar..."
}
```

---

## 👥 3. MIJOZLAR

| Metod | Endpoint | Tavsif | Rollar |
|:-----:|:---------|:-------|:------:|
| `GET` | `/clients/` | 📋 Ro'yxat | M D SA |
| `POST` | `/clients/` | ➕ Yangi mijoz | M SA |
| `GET` | `/clients/{id}/` | 📄 Batafsil + statistika | M D SA |
| `PUT` | `/clients/{id}/` | ✏️ Tahrirlash | M SA |
| `DELETE` | `/clients/{id}/` | ❌ O'chirish | SA |
| `GET` | `/clients/{id}/orders/` | 📋 Buyurtmalar tarixi | M D SA |
| `GET` | `/clients/{id}/payments/` | 💰 To'lovlar tarixi | M B SA |
| `GET` | `/clients/{id}/balance/` | 📊 Moliyaviy balans | M B SA |
| `POST` | `/clients/{id}/notes/` | 📝 Izoh qo'shish | M SA |
| `GET` | `/clients/debtors/` | 🔴 Qarzdorlar | M B D SA |

---

## 🏗 4. SKLAD

| Metod | Endpoint | Tavsif | Rollar |
|:-----:|:---------|:-------|:------:|
| `GET` | `/warehouse/products/` | 📦 Mahsulotlar | SK O SA |
| `POST` | `/warehouse/products/` | ➕ Yangi mahsulot | SK SA |
| `GET` | `/warehouse/products/{id}/` | 📄 Batafsil | SK O SA |
| `PUT` | `/warehouse/products/{id}/` | ✏️ Tahrirlash | SK SA |
| `GET` | `/warehouse/products/low-stock/` | ⚠️ Kam qolganlar | SK SA D |
| `POST` | `/warehouse/income/` | 📥 Yangi kirim | SK O SA |
| `POST` | `/warehouse/expense/` | 📤 Qo'lda chiqim | SK SA |
| `GET` | `/warehouse/movements/` | 📜 Kirim/chiqim tarixi | SK SA |
| `GET` | `/warehouse/report/` | 📊 Sklad hisoboti | SK D SA |
| `GET` | `/warehouse/suppliers/` | 🚚 Yetkazib beruvchilar | SK SA |

### 📥 Kirim yaratish

```json
// POST /warehouse/income/
{
    "product_id": "uuid",
    "quantity": 100.5,
    "unit_price": 15000,
    "supplier_id": "uuid",
    "document_number": "SF-2025-0042"
}
```

---

## 👷 5. ISHCHILAR (HR)

| Metod | Endpoint | Tavsif | Rollar |
|:-----:|:---------|:-------|:------:|
| `GET` | `/staff/workers/` | 👷 Ro'yxat | M D SA |
| `POST` | `/staff/workers/` | ➕ Yangi ishchi | SA |
| `GET` | `/staff/workers/{id}/` | 📄 Batafsil | M D SA |
| `PUT` | `/staff/workers/{id}/` | ✏️ Tahrirlash | SA |
| `GET` | `/staff/attendance/` | 📋 Davomat jurnali | M D SA |
| `GET` | `/staff/attendance/today/` | 📊 Bugungi holat | M D SA |
| `GET` | `/staff/workers/{id}/attendance/` | 📅 Ishchi davomati | M D SA |
| `GET` | `/staff/workers/{id}/salary/` | 💰 Ish haqi hisobi | B D SA |
| `POST` | `/staff/salary/calculate/` | 🧮 Oylik hisoblash | B SA |
| `GET` | `/staff/departments/` | 🏢 Bo'limlar | SA |
| `GET` | `/staff/schedules/` | 🕐 Jadvalar | SA |

---

## 🔧 6. XIZMATLAR

| Metod | Endpoint | Tavsif | Rollar |
|:-----:|:---------|:-------|:------:|
| `GET` | `/services/` | 📋 Zakazlar | M D SA |
| `POST` | `/services/` | ➕ Yangi zakaz | M SA |
| `GET` | `/services/{id}/` | 📄 Batafsil + resurslar | M D SA |
| `PUT` | `/services/{id}/` | ✏️ Tahrirlash | M SA |
| `PATCH` | `/services/{id}/status/` | 🔄 Holat o'zgartirish | M SA |
| `POST` | `/services/{id}/resources/` | ➕ Resurs qo'shish | M SA |
| `DELETE` | `/services/{id}/resources/{rid}/` | 🗑 Resurs o'chirish | M SA |
| `GET` | `/services/{id}/costs/` | 💰 Xarajat hisobi | M D B SA |

---

## 💰 7. MOLIYA

| Metod | Endpoint | Tavsif | Rollar |
|:-----:|:---------|:-------|:------:|
| `GET` | `/finance/payments/` | 💰 To'lovlar | B M D SA |
| `POST` | `/finance/payments/` | ➕ To'lov qabul | B M SA |
| `GET` | `/finance/payments/{id}/` | 📄 Batafsil | B D SA |
| `GET` | `/finance/expenses/` | 💸 Xarajatlar | B D SA |
| `POST` | `/finance/expenses/` | ➕ Xarajat kiritish | B SA |
| `GET` | `/finance/cashflow/` | 📊 Kassa hisoboti | B D SA |
| `GET` | `/finance/report/daily/` | 📅 Kunlik hisobot | B D SA |
| `GET` | `/finance/report/monthly/` | 📅 Oylik hisobot | B D SA |
| `GET` | `/finance/debts/` | 🔴 Qarzlar | B D SA |

---

## 📊 8. DASHBOARD

| Metod | Endpoint | Tavsif |
|:-----:|:---------|:-------|
| `GET` | `/dashboard/stats/` | 📊 Umumiy statistika |
| `GET` | `/dashboard/orders-today/` | 📋 Bugungi buyurtmalar |
| `GET` | `/dashboard/revenue/` | 📈 Daromad grafigi |
| `GET` | `/dashboard/staff-status/` | 👷 Ishchilar holati |
| `GET` | `/dashboard/stock-alerts/` | ⚠️ Sklad ogohlantirishlari |
| `WS` | `/ws/dashboard/` | 🔌 Real-time yangilanish |

### 📊 Stats Response

```json
{
    "orders_today": { "count": 12, "total": 15200000 },
    "active_orders": { "count": 28 },
    "rush_orders": { "count": 3 },
    "revenue_today": 8500000,
    "revenue_change_pct": 12.5,
    "staff": { "present": 12, "absent": 3, "late": 2 },
    "low_stock_count": 4
}
```

---

## 🤖 9. TELEGRAM BOT

| Metod | Endpoint | Tavsif |
|:-----:|:---------|:-------|
| `POST` | `/bot/webhook/` | Telegram webhook |
| `POST` | `/bot/attendance/checkin/` | ✅ KELDIM |
| `POST` | `/bot/attendance/checkout/` | 🚪 KETDIM |
| `PATCH` | `/bot/attendance/{id}/confirm/` | ✔️ Tasdiqlash |
| `PATCH` | `/bot/attendance/{id}/reject/` | ❌ Rad etish |

---

## 📐 10. UMUMIY KONVENTSIYALAR

### 📄 Pagination
```json
{
    "count": 150,
    "next": "...?page=2",
    "previous": null,
    "results": [...]
}
```

### ❌ Error Format
```json
{
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "Xato",
        "details": {
            "width_cm": ["Majburiy"]
        }
    }
}
```

### 🔐 Rol qisqartmalari

| Qisqartma | Rol | Qisqartma | Rol |
|:---------:|:----|:---------:|:----|
| **SA** | 👑 Super Admin | **SK** | 📦 Sklad mudiri |
| **D** | 📊 Direktor | **B** | 💰 Buxgalter |
| **M** | 📋 Menejer | **I** | 👷 Ishchi |
| **O** | ⚙️ Operator | | |

---

*🔌 API spetsifikatsiyasi yakunlandi*
