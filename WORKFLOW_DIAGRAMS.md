<![CDATA[<div align="center">

# 🔄 WORKFLOW DIAGRAMMALARI

### NafGroup CRM — Biznes Jarayonlar Oqimi

---

</div>

<br/>

## 📋 1. BUYURTMA — TO'LIQ HAYOT SIKLI

```mermaid
flowchart TD
    START(("🟢 START"))

    START --> D1["📤 Diller fayl yuklaydi<br/>(20 tagacha fayl)"]
    D1 --> D2["📝 Menejer buyurtma<br/>kartochkasini to'ldiradi"]
    D2 --> D3{"📦 Sklad tekshiruvi<br/>Material yetarlimi?"}

    D3 -->|"❌ Yetarli emas"| D4["🔔 Ogohlantirish<br/>Sklad mudiriga xabar"]
    D4 --> D5["📥 Material kirimini<br/>kutish"]
    D5 --> D3

    D3 -->|"✅ Yetarli"| D6["📋 YANGI holat<br/>Navbatga qo'shildi"]
    
    D6 --> D7["🔵 TAYYORLANMOQDA<br/>Fayl tekshiruv + Material"]
    D7 --> D8["🟡 BOSISHDA<br/>Skladdan material ayiriladi"]
    D8 --> D9["🟠 LAMINATSIYA"]
    D9 --> D10["🟤 KESISH"]
    D10 --> D11["✅ TAYYOR<br/>Ustaboshi tasdiqlaydi"]
    D11 --> D12["📦 TOPSHIRILDI<br/>Mijozga berildi"]
    D12 --> D13["💰 TO'LANDI"]
    D13 --> FINISH(("🔴 END"))

    D7 -.->|"🔴 Bekor"| CANCEL["❌ BEKOR QILINDI<br/>Sabab kiritiladi"]
    D8 -.->|"🔄 Qayta"| D7
    D9 -.->|"🔄 Qayta"| D8
    CANCEL --> FINISH

    style START fill:#00C853,color:#fff
    style FINISH fill:#D50000,color:#fff
    style CANCEL fill:#FF6D00,color:#fff
    style D11 fill:#00C853,color:#fff
    style D13 fill:#FFD600,color:#000
```

---

<br/>

## 🏗 2. SKLAD KIRIM-CHIQIM

```mermaid
flowchart LR
    subgraph IN["📥 KIRIM"]
        direction TB
        K1["🚚 Yetkazib<br/>beruvchidan"]
        K2["🔄 Qaytarish"]
        K3["📋 Inventarizatsiya<br/>tuzatish"]
    end

    subgraph STOCK["🏗 SKLAD"]
        direction TB
        S1["📦 Joriy<br/>qoldiq"]
        S2{"⚠️ Qoldiq<br/>< Minimal?"}
    end

    subgraph OUT["📤 CHIQIM"]
        direction TB
        C1["📋 Buyurtmaga<br/>(avtomatik)"]
        C2["🔧 Xizmatga<br/>(menejer tasdiqlaydi)"]
        C3["❌ Yaroqsizlik<br/>(admin + sklad)"]
        C4["✋ Qo'lda<br/>(sabab majburiy)"]
    end

    K1 & K2 & K3 -->|"+qty"| S1
    S1 -->|"−qty"| C1 & C2 & C3 & C4
    S1 --> S2
    S2 -->|"Ha"| ALERT["🔴 OGOHLANTIRISH<br/>Dashboard + Telegram"]
    S2 -->|"Yo'q"| OK["🟢 Normal"]

    style IN fill:#E8F5E9
    style STOCK fill:#FFF3E0
    style OUT fill:#FFEBEE
    style ALERT fill:#D50000,color:#fff
```

---

<br/>

## 👷 3. HR DAVOMAT — TO'LIQ JARAYON

```mermaid
sequenceDiagram
    participant I as 👷 Ishchi
    participant B as 🤖 Telegram Bot
    participant S as 🖥 Backend
    participant M as 👨‍💼 Boshliq

    rect rgb(230, 245, 230)
        Note over I,M: 🌅 ISH KUNINING BOSHLANISHI
        I->>B: ✅ KELDIM tugmasini bosadi
        B->>S: POST /attendance/checkin
        S-->>S: ⏰ Vaqt qayd + kechikish hisob
        S->>B: Boshliqqa xabar yuborish
        B->>M: 📩 "Alisher keldi (09:23, 3 daq kech)"
        
        alt ✔️ TASDIQLASH
            M->>B: ✔️ Tasdiqlash tugmasi
            B->>S: PATCH /attendance/confirm
            S->>I: ✅ "Davomatiz tasdiqlandi"
        else ❌ RAD ETISH
            M->>B: ❌ Rad etish + sabab
            B->>S: PATCH /attendance/reject
            S->>I: ❌ "Rad etildi: sabab"
        else ⏰ 10 DAQ JAVOB YO'Q
            S-->>S: ⏰ Avtomatik tasdiqlash
            S->>I: ✅ "Avtomatik tasdiqlandi"
        end
    end

    rect rgb(255, 240, 230)
        Note over I,M: 🌙 ISH KUNINING TUGASHI
        I->>B: 🚪 KETDIM tugmasini bosadi
        B->>S: POST /attendance/checkout
        S-->>S: 📊 Kunlik hisob-kitob
        S->>I: 📊 "Bugun: 8s 15d | Kechikish: 3d"
    end
```

---

<br/>

## 💰 4. OYLIK ISH HAQI HISOBLASH

```mermaid
flowchart TD
    START["📅 OY OXIRI<br/>Hisoblash boshlandi"]

    START --> LOOP["🔄 Har bir ishchi uchun:"]
    LOOP --> L1["📊 Kelgan kunlarni sanash"]
    L1 --> L2["🕐 Jami ish soatlarini hisoblash"]
    L2 --> L3["⏰ Kechikishlarni yig'ish"]
    L3 --> L4{"💼 Maosh turi?"}

    L4 -->|"💼 OYLIK"| F1["Oylik ÷ IshKunlari<br/>× KelganKunlar"]
    L4 -->|"⏰ SOATBAY"| F2["Stavka × JamiSoat"]
    L4 -->|"📅 KUNDUZGI"| F3["KunlikStavka<br/>× KelganKunlar"]

    F1 & F2 & F3 --> L5["📉 Jarimalar chiqariladi"]
    L5 --> L6["⭐ Bonuslar qo'shiladi"]
    L6 --> L7["💵 JAMI MAOSH"]
    L7 --> L8["💾 salary_records ga saqlash"]
    L8 --> L9["📄 Excel hisobot yaratish"]
    L9 --> DONE["✅ Buxgalterga yuborildi"]

    style START fill:#1565C0,color:#fff
    style DONE fill:#2E7D32,color:#fff
    style L7 fill:#FFD600,color:#000
```

---

<br/>

## 🔧 5. XIZMAT ZAKAZI JARAYONI

```mermaid
flowchart TD
    START["📞 Mijoz murojaat qildi"]

    START --> S1["📝 Menejer xizmat<br/>zakazi yaratadi"]
    S1 --> S2["📋 Tur · Manzil · Sana"]
    S2 --> S3["👷 Ishchilar tayinlanadi"]
    S3 --> S4["📅 REJALASHTIRILGAN"]

    S4 -->|"Belgilangan sanada"| S5["⚡ JARAYONDA"]
    
    S5 --> R["📦 Resurslar kiritiladi:"]
    R --> R1["🚛 Texnika"]
    R --> R2["🪜 Lesa"]
    R --> R3["📦 Material (skladdan)"]
    R --> R4["💸 Mayda xarajatlar"]
    R --> R5["👷 Ishchi kuchi"]

    R1 & R2 & R3 & R4 & R5 --> CALC["🧮 JAMI XARAJAT = ΣResurslar"]
    CALC --> PROFIT["📈 FOYDA = Shartnoma − Xarajat"]
    PROFIT --> S6["✅ YAKUNLANDI"]
    S6 --> S7["💰 To'lov qabul qilindi"]

    style START fill:#1565C0,color:#fff
    style CALC fill:#FFD600,color:#000
    style PROFIT fill:#2E7D32,color:#fff
```

---

<br/>

## 💳 6. TO'LOV VA QARZ BOSHQARUVI

```mermaid
flowchart TD
    BUY["📋 Buyurtma / Xizmat<br/>💰 Narx: 10M UZS"]

    BUY --> T{"💳 To'lov turi?"}

    T -->|"💰 To'liq"| FULL["✅ 10M to'landi<br/>Holat: TO'LANDI"]

    T -->|"💵 Qisman"| P1["💵 Avans: 4M<br/>📊 Qoldiq: 6M"]
    P1 --> P2{"Keyingi to'lov?"}
    P2 -->|"💵 Ha"| P3["💵 +3M to'landi<br/>📊 Qoldiq: 3M"]
    P3 --> P2
    P2 -->|"Hammasi to'landi"| FULL

    T -->|"📝 Nasiya"| N1["📝 Qarz qayd qilinadi<br/>⏰ Muddat belgilanadi"]
    N1 --> N2{"⏰ Muddat o'tdimi?"}
    N2 -->|"Ha"| N3["🔴 Ogohlantirish<br/>Mijoz → MUAMMOLI"]
    N2 -->|"Yo'q"| N4["🟡 Kutish"]
    N4 --> N2
    N3 --> P2

    FULL --> DONE["✅ YAKUNLANDI"]

    style BUY fill:#1565C0,color:#fff
    style FULL fill:#2E7D32,color:#fff
    style N3 fill:#D50000,color:#fff
```

---

<br/>

## 🔐 7. LOGIN VA NAVIGATSIYA

```mermaid
flowchart TD
    START["🌐 Foydalanuvchi<br/>saytga kiradi"]
    
    START --> L1["📝 Login: Email + Parol"]
    L1 --> L2{"🔑 Autentifikatsiya?"}

    L2 -->|"❌ Xato"| L3["🔴 Xato xabari"]
    L3 --> L1

    L2 -->|"✅ Muvaffaqiyat"| L4["🔑 JWT token olinadi"]
    L4 --> L5{"👤 Rol?"}

    L5 -->|"👑 SA / 📊 Dir"| D1["📊 To'liq Dashboard<br/>Barcha modullar"]
    L5 -->|"📋 Menejer"| D2["📋 Buyurtmalar<br/>Mijozlar · Xizmatlar"]
    L5 -->|"⚙️ Operator"| D3["⚙️ Ishlab chiqarish<br/>Status o'zgartirish"]
    L5 -->|"📦 Sklad"| D4["📦 Sklad panel<br/>Kirim/Chiqim"]
    L5 -->|"💰 Buxgalter"| D5["💰 Moliya panel<br/>Hisobotlar"]

    D1 & D2 & D3 & D4 & D5 --> NAV["📱 Sidebar navigatsiya<br/>Faqat ruxsat berilgan modullar"]

    style START fill:#1565C0,color:#fff
    style L4 fill:#2E7D32,color:#fff
    style L3 fill:#D50000,color:#fff
```

---

<div align="center">

*🔄 Workflow diagrammalari yakunlandi*

</div>
]]>
