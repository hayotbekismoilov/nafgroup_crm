# 🗄 MA'LUMOTLAR BAZASI SXEMASI
### NafGroup CRM — PostgreSQL Database Schema

---

## 📊 1. ER DIAGRAMMA

```mermaid
erDiagram
    USERS ||--o{ WORKERS : "profile"
    USERS {
        uuid id PK
        varchar email UK
        varchar password
        varchar full_name
        varchar role
        boolean is_active
        timestamp created_at
    }

    WORKERS ||--o{ ATTENDANCE : "records"
    WORKERS {
        uuid id PK
        uuid user_id FK
        varchar full_name
        bigint telegram_id UK
        uuid position_id FK
        uuid department_id FK
        varchar salary_type
        decimal salary_amount
        uuid schedule_id FK
        varchar status
    }

    CLIENTS ||--o{ ORDERS : "places"
    CLIENTS ||--o{ SERVICE_ORDERS : "requests"
    CLIENTS ||--o{ PAYMENTS : "pays"
    CLIENTS {
        uuid id PK
        varchar organization
        varchar contact_name
        jsonb phones
        varchar category
        timestamp created_at
    }

    ORDERS ||--o{ ORDER_FILES : "has"
    ORDERS ||--o{ STATUS_HISTORY : "logs"
    ORDERS ||--o{ PAYMENTS : "receives"
    ORDERS {
        uuid id PK
        varchar order_number UK
        uuid client_id FK
        decimal width_cm
        decimal height_cm
        integer quantity
        uuid material_id FK
        varchar priority
        decimal price
        varchar status
        timestamp deadline
    }

    PRODUCTS ||--o{ STOCK_MOVES : "tracked"
    PRODUCTS {
        uuid id PK
        varchar name
        varchar sku UK
        uuid category_id FK
        decimal current_stock
        decimal min_stock
        decimal purchase_price
    }

    SERVICE_ORDERS ||--o{ SVC_RESOURCES : "uses"
    SERVICE_ORDERS ||--o{ PAYMENTS : "receives"
    SERVICE_ORDERS {
        uuid id PK
        varchar service_type
        uuid client_id FK
        decimal contract_price
        decimal total_cost
        varchar status
    }

    PAYMENTS {
        uuid id PK
        uuid client_id FK
        uuid order_id FK
        decimal amount
        varchar method
        timestamp paid_at
    }
```

---

## 📋 2. JADVALLAR RO'YXATI

| # | Jadval | Modul | Tavsif |
|:-:|:-------|:-----:|:-------|
| 1 | `users` | 🔑 Core | Foydalanuvchilar va autentifikatsiya |
| 2 | `workers` | 👷 HR | Ishchilar profili |
| 3 | `departments` | 👷 HR | Bo'limlar |
| 4 | `positions` | 👷 HR | Lavozimlar |
| 5 | `schedules` | 👷 HR | Ish jadvallari |
| 6 | `attendance_logs` | 👷 HR | Keldi-ketdi jurnali |
| 7 | `salary_records` | 👷 HR | Oylik ish haqi |
| 8 | `clients` | 👥 CRM | Mijozlar |
| 9 | `client_tags` | 👥 CRM | Teglar |
| 10 | `orders` | 📋 Orders | Buyurtmalar |
| 11 | `order_files` | 📋 Orders | Buyurtma fayllari |
| 12 | `order_status_history` | 📋 Orders | Holat tarixi |
| 13 | `products` | 🏗 Sklad | Mahsulotlar |
| 14 | `product_categories` | 🏗 Sklad | Kategoriyalar |
| 15 | `suppliers` | 🏗 Sklad | Yetkazib beruvchilar |
| 16 | `stock_movements` | 🏗 Sklad | Kirim/chiqim harakatlari |
| 17 | `service_orders` | 🔧 Xizmat | Xizmat zakazlari |
| 18 | `service_resources` | 🔧 Xizmat | Xizmat resurslari |
| 19 | `payments` | 💰 Moliya | To'lovlar |
| 20 | `expenses` | 💰 Moliya | Xarajatlar |
| 21 | `expense_categories` | 💰 Moliya | Xarajat toifalari |
| 22 | `audit_log` | 📝 Core | Amallar logi |
| 23 | `settings` | ⚙️ Core | Tizim sozlamalari |

---

## 🔨 3. SQL DDL — ASOSIY JADVALLAR

### 3.1 🔑 `users`

```sql
CREATE TABLE users (
    id          UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    email       VARCHAR(255) UNIQUE NOT NULL,
    password    VARCHAR(255) NOT NULL,
    full_name   VARCHAR(255) NOT NULL,
    phone       VARCHAR(20),
    role        VARCHAR(20) NOT NULL DEFAULT 'WORKER',
    -- Roles: SUPERADMIN | DIRECTOR | MANAGER | OPERATOR
    --        WAREHOUSE_MANAGER | ACCOUNTANT | WORKER
    is_active   BOOLEAN DEFAULT TRUE,
    last_login  TIMESTAMPTZ,
    created_at  TIMESTAMPTZ DEFAULT NOW(),
    updated_at  TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_role ON users(role);
```

---

### 3.2 👷 `workers`

```sql
CREATE TABLE workers (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    user_id           UUID REFERENCES users(id) ON DELETE CASCADE,
    full_name         VARCHAR(255) NOT NULL,
    phone             VARCHAR(20) NOT NULL,
    telegram_id       BIGINT UNIQUE,
    telegram_username VARCHAR(100),
    position_id       UUID REFERENCES positions(id),
    department_id     UUID REFERENCES departments(id),
    hire_date         DATE NOT NULL,
    salary_type       VARCHAR(10) NOT NULL,  -- MONTHLY | HOURLY | DAILY
    salary_amount     DECIMAL(15,2) NOT NULL,
    schedule_id       UUID REFERENCES schedules(id),
    bank_details      TEXT,
    status            VARCHAR(20) DEFAULT 'ACTIVE',
    -- ACTIVE | ON_LEAVE | SICK | DISMISSED
    photo             VARCHAR(500),
    created_at        TIMESTAMPTZ DEFAULT NOW(),
    updated_at        TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_workers_telegram ON workers(telegram_id);
CREATE INDEX idx_workers_status ON workers(status);
```

---

### 3.3 📋 `orders`

```sql
CREATE TABLE orders (
    id               UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    order_number     VARCHAR(30) UNIQUE NOT NULL,  -- ORD-20250402-001
    client_id        UUID NOT NULL REFERENCES clients(id),
    width_cm         DECIMAL(10,2) NOT NULL,
    height_cm        DECIMAL(10,2) NOT NULL,
    measurement_unit VARCHAR(5) DEFAULT 'cm',      -- mm | cm | m
    quantity         INTEGER NOT NULL DEFAULT 1,
    material_id      UUID REFERENCES products(id),
    print_type       VARCHAR(20) DEFAULT 'ONE_SIDE', -- ONE_SIDE | TWO_SIDE
    submitted_at     TIMESTAMPTZ DEFAULT NOW(),
    deadline         TIMESTAMPTZ NOT NULL,
    priority         VARCHAR(15) DEFAULT 'NORMAL', -- NORMAL | URGENT | RUSH
    queue_position   INTEGER,
    price            DECIMAL(15,2) NOT NULL,
    material_cost    DECIMAL(15,2) DEFAULT 0,
    notes            TEXT,
    status           VARCHAR(20) DEFAULT 'NEW',
    -- NEW | PREPARING | PRINTING | LAMINATING | CUTTING
    -- READY | DELIVERED | PAID | CANCELLED
    created_by       UUID REFERENCES users(id),
    assigned_to      UUID REFERENCES users(id),
    created_at       TIMESTAMPTZ DEFAULT NOW(),
    updated_at       TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_orders_status   ON orders(status);
CREATE INDEX idx_orders_client   ON orders(client_id);
CREATE INDEX idx_orders_deadline ON orders(deadline);
CREATE INDEX idx_orders_priority ON orders(priority);
CREATE INDEX idx_orders_queue    ON orders(queue_position);
CREATE INDEX idx_orders_created  ON orders(created_at DESC);
```

---

### 3.4 👥 `clients`

```sql
CREATE TABLE clients (
    id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    organization_name VARCHAR(255),
    contact_name      VARCHAR(255) NOT NULL,
    contact_position  VARCHAR(100),
    phone_numbers     JSONB NOT NULL DEFAULT '[]',
    email             VARCHAR(255),
    telegram          VARCHAR(100),
    tax_id            VARCHAR(20),       -- STIR
    requisites        TEXT,
    address           TEXT,
    category          VARCHAR(15) DEFAULT 'NEW',
    -- VIP | REGULAR | NEW | PROBLEM
    notes             TEXT,
    registered_at     DATE DEFAULT CURRENT_DATE,
    last_activity     DATE,
    created_at        TIMESTAMPTZ DEFAULT NOW(),
    updated_at        TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_clients_category ON clients(category);
CREATE INDEX idx_clients_name ON clients(contact_name);
```

---

### 3.5 🏗 `products` + `stock_movements`

```sql
CREATE TABLE products (
    id             UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name           VARCHAR(255) NOT NULL,
    sku            VARCHAR(50) UNIQUE NOT NULL,
    category_id    UUID REFERENCES product_categories(id),
    unit           VARCHAR(10) NOT NULL,  -- m2 | piece | kg | meter | liter | roll
    current_stock  DECIMAL(15,3) DEFAULT 0,
    min_stock      DECIMAL(15,3) DEFAULT 0,
    purchase_price DECIMAL(15,2) DEFAULT 0,
    selling_price  DECIMAL(15,2) DEFAULT 0,
    supplier_id    UUID REFERENCES suppliers(id),
    location       VARCHAR(100),
    is_active      BOOLEAN DEFAULT TRUE,
    created_at     TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE stock_movements (
    id               UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    product_id       UUID NOT NULL REFERENCES products(id),
    movement_type    VARCHAR(15) NOT NULL,
    -- INCOME | ORDER_EXPENSE | SERVICE_EXPENSE | MANUAL_EXPENSE | RETURN
    quantity         DECIMAL(15,3) NOT NULL,
    unit_price       DECIMAL(15,2),
    total_amount     DECIMAL(15,2),
    order_id         UUID REFERENCES orders(id),
    service_order_id UUID REFERENCES service_orders(id),
    supplier_id      UUID REFERENCES suppliers(id),
    document_number  VARCHAR(50),
    document_file    VARCHAR(500),
    reason           TEXT,
    performed_by     UUID NOT NULL REFERENCES users(id),
    performed_at     TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_stock_product ON stock_movements(product_id);
CREATE INDEX idx_stock_date    ON stock_movements(performed_at DESC);
```

---

### 3.6 💰 `payments`

```sql
CREATE TABLE payments (
    id               UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    payment_number   VARCHAR(30) UNIQUE NOT NULL,  -- PAY-20250402-001
    order_id         UUID REFERENCES orders(id),
    service_order_id UUID REFERENCES service_orders(id),
    client_id        UUID NOT NULL REFERENCES clients(id),
    amount           DECIMAL(15,2) NOT NULL,
    payment_method   VARCHAR(20) NOT NULL,
    -- CASH | BANK_TRANSFER | CARD | PAYME | CLICK | UZUM
    receipt_number   VARCHAR(50),
    receipt_file     VARCHAR(500),
    notes            TEXT,
    received_by      UUID REFERENCES users(id),
    paid_at          TIMESTAMPTZ DEFAULT NOW()
);

CREATE INDEX idx_payments_date   ON payments(paid_at DESC);
CREATE INDEX idx_payments_client ON payments(client_id);
```

---

## ⚡ 4. TRIGGER — SKLAD AVTOMATIK YANGILANISH

```sql
CREATE OR REPLACE FUNCTION update_stock()
RETURNS TRIGGER AS $$
BEGIN
    IF NEW.movement_type IN ('INCOME', 'RETURN') THEN
        UPDATE products 
        SET current_stock = current_stock + NEW.quantity,
            updated_at = NOW()
        WHERE id = NEW.product_id;
    ELSE
        UPDATE products 
        SET current_stock = current_stock - NEW.quantity,
            updated_at = NOW()
        WHERE id = NEW.product_id;
    END IF;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER trg_stock_movement
AFTER INSERT ON stock_movements
FOR EACH ROW EXECUTE FUNCTION update_stock();
```

---

## 🔗 5. BOG'LIQLIKLAR DIAGRAMMASI

```mermaid
graph TB
    subgraph Core["🔐 Core"]
        U["users"]
        S["settings"]
        AL["audit_log"]
    end
    subgraph HR["👷 HR"]
        W["workers"]
        ATT["attendance"]
        SAL["salary"]
    end
    subgraph CRM["👥 CRM"]
        CL["clients"]
        CT["tags"]
    end
    subgraph Orders["📋 Orders"]
        O["orders"]
        OF["order_files"]
        OH["status_history"]
    end
    subgraph Warehouse["🏗 Warehouse"]
        PR["products"]
        SM["stock_moves"]
    end
    subgraph Services["🔧 Services"]
        SO["service_orders"]
        SR["resources"]
    end
    subgraph Finance["💰 Finance"]
        PAY["payments"]
        EXP["expenses"]
    end

    U --> W & O & SM & PAY & AL
    W --> ATT & SAL & SO
    CL --> O & SO & PAY
    O --> OF & OH & SM & PAY
    PR --> SM
    SO --> SR & PAY

    style Core fill:#FFF3E0
    style HR fill:#E3F2FD
    style CRM fill:#F3E5F5
    style Orders fill:#E8F5E9
    style Warehouse fill:#FFFDE7
    style Services fill:#FCE4EC
    style Finance fill:#E0F2F1
```

---

*🗄 Ma'lumotlar bazasi sxemasi yakunlandi*
