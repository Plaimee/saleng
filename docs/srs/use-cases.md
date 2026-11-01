# สารบัญ Use Cases ทั้งระบบ (`docs/srs/use-cases.md`) — โครงการ Saleng (กลุ่ม 2)

เอกสารนี้สรุป Use Cases ทั้ง 13 ข้อ (`UC-01..UC-13`) ของแพลตฟอร์มรับซื้อของเก่า Saleng และเชื่อมโยงไปยังสเปกแยกตามบทบาทในโฟลเดอร์ [`specs/`](../../specs/)

---

## ตารางสรุป Use Cases (`UC-01` ถึง `UC-13`)

| รหัส UC | ชื่อ Use Case | ผู้ใช้งานหลัก (Primary Actor) | สถานะใบงานที่เกี่ยวข้อง (`S1..S9`) | สเปกฟีเจอร์ที่รองรับใน `specs/` |
|---|---|---|---|---|
| `UC-01` | ลงทะเบียน เข้าสู่ระบบ และยืนยันตัวตน (KYC / บัญชีผู้ใช้) | ผู้ขาย (`SH-02`), ซาเล้ง (`SH-01`) | ก่อนเริ่มใบงาน | [`specs/seller/01-account-and-access/`](../../specs/seller/01-account-and-access/spec.md), [`specs/buyer/01-account-and-kyc/`](../../specs/buyer/01-account-and-kyc/spec.md) |
| `UC-02` | โพสต์ขายของเก่า ประเมินน้ำหนัก และอัปโหลดรูปถ่าย | ผู้ขาย (`SH-02`) | `[* -> S1 Posted]` | [`specs/seller/02-scrap-posting/`](../../specs/seller/02-scrap-posting/spec.md), [`specs/seller/04-pricing-and-decision/`](../../specs/seller/04-pricing-and-decision/spec.md) |
| `UC-03` | ค้นหา เลือกผู้รับซื้อ และยืนยันนัดหมายพร้อมล็อกราคากลาง | ผู้ขาย (`SH-02`), ซาเล้ง (`SH-01`) | `S1 -> S2 Matched` | [`specs/seller/03-buyer-search-and-selection/`](../../specs/seller/03-buyer-search-and-selection/spec.md), [`specs/buyer/02-job-search-and-map/`](../../specs/buyer/02-job-search-and-map/spec.md), [`specs/buyer/03-pre-acceptance-evaluation/`](../../specs/buyer/03-pre-acceptance-evaluation/spec.md) |
| **`UC-04`** | **จัดการคิวงาน คำนวณเส้นทางรับซื้อ และติดตามสถานะการเดินทาง (ฟีเจอร์หลัก SDD)** | **ซาเล้ง (`SH-01`), ผู้ขาย (`SH-02`)** | **`S2 -> S3 -> S4`** | **[`specs/002-route/spec.md`](../../specs/002-route/spec.md), [`specs/SPEC-ROUTE/spec.md`](../../specs/SPEC-ROUTE/spec.md), [`specs/buyer/04-job-and-route-management/`](../../specs/buyer/04-job-and-route-management/spec.md)** |
| `UC-05` | ชั่งน้ำหนักจริงที่หน้างาน ถ่ายภาพตาชั่ง และตรวจสอบเกณฑ์ส่วนต่าง `±15%` | ซาเล้ง (`SH-01`) | `S4 -> S5` หรือ `S4 -> S6` | [`specs/002-route/spec.md`](../../specs/002-route/spec.md), [`specs/seller/05-booking-and-status-tracking/`](../../specs/seller/05-booking-and-status-tracking/spec.md) |
| `UC-06` | ยืนยันหรือปฏิเสธใบเสนอราคาใหม่เมื่อน้ำหนักชั่งจริงต่างเกิน `±15%` | ผู้ขาย (`SH-02`) | `S5 -> S6` หรือ `S5 -> S8` | [`specs/002-route/spec.md`](../../specs/002-route/spec.md), [`specs/seller/04-pricing-and-decision/`](../../specs/seller/04-pricing-and-decision/spec.md) |
| `UC-07` | ชำระเงิน ออกใบเสร็จดิจิทัล และซิงก์ข้อมูลจากคิวออฟไลน์ | ซาเล้ง (`SH-01`), ผู้ขาย (`SH-02`) | `S6 -> S7 Completed` | [`specs/seller/06-payment-and-verification/`](../../specs/seller/06-payment-and-verification/spec.md), [`specs/buyer/06-billing-and-receipt/`](../../specs/buyer/06-billing-and-receipt/spec.md) |
| `UC-08` | สื่อสารผ่านข้อความมาตรฐานและแจ้งเตือนสถานะนัดหมาย | ซาเล้ง (`SH-01`), ผู้ขาย (`SH-02`) | `S2..S7` | [`specs/seller/08-notifications-and-help/`](../../specs/seller/08-notifications-and-help/spec.md), [`specs/buyer/05-communication/`](../../specs/buyer/05-communication/spec.md) |
| `UC-09` | ดูแลความปลอดภัยขณะทำธุรกรรมและปุ่มฉุกเฉิน SOS | ผู้ขาย (`SH-02`), ซาเล้ง (`SH-01`) | `S3..S6` | [`specs/seller/09-transaction-safety/`](../../specs/seller/09-transaction-safety/spec.md) |
| `UC-10` | ให้คะแนนรีวิว สรุปยอดรายวัน และจัดการหลังจบงาน | ผู้ขาย (`SH-02`), ซาเล้ง (`SH-01`) | `S7 Completed` | [`specs/seller/07-post-transaction/`](../../specs/seller/07-post-transaction/spec.md), [`specs/buyer/07-post-job/`](../../specs/buyer/07-post-job/spec.md) |
| `UC-11` | ตรวจสอบอนุมัติ KYC และจัดการบทลงโทษผู้ใช้งาน | ผู้ดูแลระบบ (`SH-04`) | ทุกสถานะ | [`specs/admin/01-user-verification-and-penalty/`](../../specs/admin/01-user-verification-and-penalty/spec.md) |
| `UC-12` | จัดการราคากลางวัสดุรีไซเคิลและเกณฑ์การคัดเกรด | ผู้ดูแลระบบ (`SH-04`) | ก่อน `S1..S2` | [`specs/admin/02-benchmark-price-and-grading/`](../../specs/admin/02-benchmark-price-and-grading/spec.md) |
| `UC-13` | ตัดสินข้อพิพาท (`S8`) และตรวจสอบ Audit Log / PDPA Compliance | ผู้ดูแลระบบ (`SH-04`) | `S8 -> S7` หรือ `S9` | [`specs/admin/03-dispatch-and-dispute-center/`](../../specs/admin/03-dispatch-and-dispute-center/spec.md), [`specs/admin/04-audit-security-and-compliance/`](../../specs/admin/04-audit-security-and-compliance/spec.md) |
