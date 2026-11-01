# โมเดลระบบและแผนภาพ (`models.md`) — ฟีเจอร์ `UC-04` การจัดการงานและเส้นทางของซาเล้ง

เอกสารนี้ประกอบด้วยโมเดลทั้ง 6 มุมมองของฟีเจอร์ `UC-04` (`MD-CTX-04`, `MD-UCD-04`, `MD-ACT-04`, `MD-STM-04`, `MD-DOM-04`, `MD-SEQ-04`) ที่สอดคล้องกับ `spec.md`, `rules.md`, และ `route.feature` ทุกประการ

---

## 1. Context Diagram (`MD-CTX-04`)

```mermaid
flowchart LR
    Seller["ผู้ขายของเก่าตามครัวเรือน (SH-02)"]
    Buyer["ซาเล้ง / ผู้รับซื้อรายย่อย (SH-01)"]
    Admin["ผู้ดูแลระบบและศูนย์ข้อพิพาท (SH-04)"]
    Yard["ลานรับซื้อของเก่าพันธมิตร (SH-03)"]
    MapsAPI["บริการแผนที่ภายนอก (OSRM / Maps API)"]
    PaymentGW["ผู้ให้บริการชำระเงิน / PromptPay"]

    subgraph SalengSystem["แพลตฟอร์มรับซื้อของเก่า Saleng (กลุ่ม 2)"]
        Core["ระบบจัดการงาน เส้นทาง การชั่งจริง และใบเสร็จดิจิทัล (UC-04)"]
    end

    Seller -- "โพสต์ของเก่า / ยืนยันราคาใหม่ (S5)" --> Core
    Core -- "แจ้งเตือน ETA / ใบเสนอราคาใหม่ / ใบเสร็จ" --> Seller
    Buyer -- "รับงาน / นำทาง / บันทึกน้ำหนักและภาพตาชั่ง" --> Core
    Core -- "คิวงาน / เส้นทาง / คิวออฟไลน์" --> Buyer
    Admin -- "อัปเดตราคากลาง / ตัดสินข้อพิพาท (S8)" --> Core
    Core -- "รายงานน้ำหนักรวมเตรียมส่งขาย" --> Yard
    Core <-- "ขอระยะทางและเส้นทาง (Fallback Haversine)" --> MapsAPI
    Core <-- "สร้าง QR / ยืนยันสลิปชำระเงิน" --> PaymentGW
```

---

## 2. Use Case Diagram (`MD-UCD-04`)

```mermaid
flowchart TB
    Buyer["ซาเล้ง (SH-01)"]
    Seller["ผู้ขายของเก่า (SH-02)"]
    Admin["ผู้ดูแลระบบ (SH-04)"]

    subgraph UC04Boundary["ขอบเขตระบบจัดการงานและเส้นทาง (UC-04)"]
        UC04_1["ค้นหาใบงานในรัศมีและตรวจสอบความจุรถ (REQ-FN-001)"]
        UC04_2["จัดลำดับคิวงานและคำนวณเส้นทาง + Fallback (REQ-FN-003, REQ-OP-002)"]
        UC04_3["บันทึกน้ำหนักชั่งจริงและภาพถ่ายหน้าตาชั่ง (REQ-FN-002)"]
        UC04_4["ส่งใบเสนอราคาใหม่เมื่อน้ำหนักต่างเกิน ±15% (REQ-FN-004)"]
        UC04_5["ชำระเงินและออกใบเสร็จดิจิทัลพร้อมคิวออฟไลน์ (REQ-FN-005, REQ-OP-001)"]
        UC04_6["แจ้งและตรวจสอบข้อพิพาทหน้างาน (REQ-FN-006)"]
    end

    Buyer --> UC04_1
    Buyer --> UC04_2
    Buyer --> UC04_3
    Buyer --> UC04_5
    UC04_3 -. "<<extend>> เมื่อส่วนต่าง > ±15%" .-> UC04_4
    Seller --> UC04_4
    Seller --> UC04_6
    Admin --> UC04_6
```

---

## 3. Activity Diagram — กระบวนการชั่งน้ำหนักจริงและตรวจสอบส่วนต่าง ±15% (`MD-ACT-04`)

```mermaid
flowchart TD
    Start(["ซาเล้งเดินทางถึงหน้างาน (S3 -> S4)"]) --> Weigh["ชั่งน้ำหนักวัสดุจริงและถ่ายภาพหน้าตาชั่ง"]
    Weigh --> CheckOffline{"มีสัญญาณอินเทอร์เน็ตหรือไม่?"}
    CheckOffline -- "ไม่มี (Offline)" --> SaveQueue["บันทึกลง Offline Queue พร้อม idempotency_key (REQ-OP-001)"]
    CheckOffline -- "มี (Online)" --> CalcVar["คำนวณส่วนต่างน้ำหนักเทียบกับที่ประเมิน (BR-02)"]
    SaveQueue --> CalcVar
    CalcVar --> Check15{"ส่วนต่างเกิน ±15% หรือไม่?"}
    Check15 -- "ไม่เกิน (<= 15%)" --> Settle["คำนวณยอดตาม Price Snapshot และเปลี่ยนเป็น S6 (Settled)"]
    Check15 -- "เกิน (> 15%)" --> ReQuote["เปลี่ยนสถานะเป็น S5 (Re-Quotation Pending) แจ้งผู้ขาย"]
    ReQuote --> SellerDecision{"ผู้ขายยอมรับน้ำหนักและยอดใหม่หรือไม่?"}
    SellerDecision -- "ยอมรับ" --> Settle
    SellerDecision -- "ปฏิเสธ" --> Dispute["เปลี่ยนสถานะเป็น S8 (Disputed) และล็อกหลักฐานส่ง Admin"]
    Settle --> IssueReceipt["ออกใบเสร็จดิจิทัลและเปลี่ยนเป็น S7 (Completed)"]
    IssueReceipt --> MaskPDPA["ปกปิดพิกัดบ้านเลขที่และเบอร์โทรหลังจบงาน (REQ-SEC-002)"]
    MaskPDPA --> End(["จบกระบวนการ"])
    Dispute --> End
```

---

## 4. State Machine Diagram — วงจรชีวิตใบงาน `S1..S9` (`MD-STM-04`)

```mermaid
stateDiagram-v2
    [*] --> S1_Posted: ผู้ขายสร้างโพสต์ขายของเก่า
    S1_Posted --> S2_Matched: ซาเล้งกดรับงานและล็อกราคากลาง (BR-01, BR-03)
    S1_Posted --> S9_Cancelled: ผู้ขายยกเลิกก่อนมีผู้รับงาน
    S2_Matched --> S3_EnRoute: ซาเล้งเริ่มออกเดินทางตามลำดับคิว (AC-04-01)
    S2_Matched --> S9_Cancelled: ยกเลิกนัดหมายตามเงื่อนไขเวลา
    S3_EnRoute --> S4_Weighing: ถึงจุดนัดหมายและเริ่มชั่งน้ำหนักจริง (AC-04-03)
    S4_Weighing --> S6_Settled: ส่วนต่างน้ำหนัก <= ±15% ชำระเงินทันที (AC-04-04, AC-04-05)
    S4_Weighing --> S5_ReQuotePending: ส่วนต่างน้ำหนัก > ±15% รอผู้ขายยืนยันใหม่ (AC-04-06)
    S5_ReQuotePending --> S6_Settled: ผู้ขายกดยืนยันราคาตามน้ำหนักชั่งจริง (AC-04-07)
    S5_ReQuotePending --> S8_Disputed: ผู้ขายปฏิเสธผลชั่ง / แจ้งข้อพิพาท (AC-04-08)
    S6_Settled --> S7_Completed: ออกใบเสร็จดิจิทัลและซิงก์ข้อมูลสมบูรณ์ (AC-04-09)
    S8_Disputed --> S7_Completed: Admin ตัดสินข้อพิพาทและปิดงาน
    S8_Disputed --> S9_Cancelled: Admin ตัดสินยกเลิกรายการ
    S7_Completed --> [*]
    S9_Cancelled --> [*]
```

---

## 5. Domain Model (`MD-DOM-04`)

```mermaid
classDiagram
    class PickupJob {
        +String job_id
        +String seller_id
        +String buyer_id
        +JobState status "S1..S9"
        +Float est_total_weight_kg
        +Float actual_total_weight_kg
        +Float variance_pct
        +Float locked_unit_price
        +Float total_amount_baht
        +String scale_photo_url
        +String masked_address
        +DateTime matched_at
        +DateTime settled_at
    }

    class RoutePlan {
        +String route_id
        +String buyer_id
        +Float max_capacity_kg
        +Float current_load_kg
        +Boolean is_fallback_haversine
        +List~RouteStop~ stops
    }

    class RouteStop {
        +String stop_id
        +String job_id
        +Int sequence_no
        +Float distance_km
        +Int eta_minutes
    }

    class DigitalReceipt {
        +String receipt_id
        +String job_id
        +String idempotency_key
        +Float final_weight_kg
        +Float locked_price_per_kg
        +Float net_total_baht
        +DateTime issued_at
        +String sync_status "SYNCED | PENDING_OFFLINE"
    }

    class AuditLogEntry {
        +String log_id
        +String job_id
        +String actor_id
        +String from_state
        +String to_state
        +String payload_hash
        +DateTime created_at
    }

    RoutePlan "1" *-- "0..*" RouteStop : จัดลำดับจุดแวะ
    RouteStop "1" --> "1" PickupJob : อ้างอิงใบงาน
    PickupJob "1" --> "0..1" DigitalReceipt : ออกใบเสร็จเมื่อ S6/S7
    PickupJob "1" --> "1..*" AuditLogEntry : บันทึกแบบแก้ไขไม่ได้
```

---

## 6. Sequence Diagram — การชั่งจริง โหมดออฟไลน์ และการซิงก์ใบเสร็จ (`MD-SEQ-04`)

```mermaid
sequenceDiagram
    actor Buyer as ซาเล้ง (SH-01)
    actor Seller as ผู้ขาย (SH-02)
    participant App as แอปซาเล้ง (Offline-First UI)
    participant LocalQ as คิวออฟไลน์ในเครื่อง (SQLite/IndexedDB)
    participant API as Saleng Backend API
    participant Maps as บริการแผนที่ (OSRM)

    Buyer->>App: กดคำนวณเส้นทางคิวงาน (S2 -> S3)
    App->>API: POST /api/routes/optimize
    API->>Maps: ขอระยะทางถนนจริง
    alt บริการแผนที่ปกติ
        Maps-->>API: ส่งคืนลำดับจุดแวะและ ETA
    else บริการแผนที่ล่ม / Timeout (AC-04-10)
        API->>API: คำนวณ Haversine Fallback * 1.35
    end
    API-->>App: แสดงแผนที่และลำดับจุดแวะ (UI-04-02)

    Buyer->>App: ถึงหน้างานและบันทึกน้ำหนักจริง + ภาพตาชั่ง (S4)
    alt น้ำหนักต่างเกิน ±15% (AC-04-06)
        App->>API: POST /api/jobs/{id}/weigh (variance > 15%)
        API-->>Seller: แจ้งเตือนใบเสนอราคาใหม่ (S5)
        Seller->>API: กดยอมรับยอดเงินใหม่ (AC-04-07)
    end

    alt ขณะออกใบเสร็จไม่มีสัญญาณเน็ต (AC-04-09)
        App->>LocalQ: บันทึกธุรกรรม + idempotency_key ลงคิวออฟไลน์
        LocalQ-->>Buyer: แสดงใบเสร็จดิจิทัลพร้อมสถานะ "รอซิงก์" (UI-04-03)
        Note over App,API: เมื่อสัญญาณอินเทอร์เน็ตกลับมาเชื่อมต่อ
        LocalQ->>API: POST /api/receipts/sync (idempotency_key)
        API->>API: ตรวจสอบ idempotency_key ป้องกันบันทึกซ้ำ
        API-->>LocalQ: 200 OK (อัปเดตสถานะเป็น SYNCED, S7)
    end
```
