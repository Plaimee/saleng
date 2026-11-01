# บันทึกผลการรันทดสอบตามเกณฑ์การยอมรับ (`ac-results.md`) — ฟีเจอร์ `UC-04`

เอกสารนี้บันทึกผลการตรวจสอบทั้ง **การรันตรวจด้วยชุดทดสอบอัตโนมัติ (Automated Tests)** และ **การอ่านตรวจหน้าจอเทียบกับ Mockup** สำหรับฟีเจอร์ `UC-04` การจัดการงานและเส้นทางของซาเล้ง

---

## 1. สรุปผลการรันทดสอบรวมแต่ละรอบ

| รอบการทดสอบ | วันที่รัน | คำสั่งที่ใช้รัน | จำนวน AC ทั้งหมด | ผ่าน (Passed) | ไม่ผ่าน (Failed) | ข้าม (Skipped) | ผลการตรวจหน้าจอเทียบ Mockup |
|---|---|---|---:|---:|---:|---:|---|
| **รอบที่ 1 (Round 1 — ก่อนแก้ GAP)** | 2026-03-08 | `pytest -v -k "AC_"` | 12 | 10 | 2 (`AC-04-05`, `AC-04-09`) | 0 | ยังไม่ได้ตรวจหน้าจอ |
| **รอบที่ 2 (Round 2 — หลังแก้ GAP-01, GAP-02)** | 2026-03-08 | `pytest -v -k "AC_"` | 12 | **12** | **0** | 0 | ตรงครบ 18/18 ข้อตรวจ (3 หน้าจอ) |

---

## 2. ตารางผลการทดสอบรายข้อ (`AC-04-01` ถึง `AC-04-12`)

| รหัส AC | ความต้องการที่รองรับ | ชื่อฟังก์ชันทดสอบอัตโนมัติ | ผลรอบที่ 1 | ผลรอบที่ 2 | หมายเหตุ / รหัส GAP ที่เกี่ยวข้อง |
|---|---|---|---|---|---|
| `AC-04-01` | `REQ-FN-001`, `REQ-UX-001` | `test_AC_04_01_route_optimization_and_masked_feed` | PASSED | **PASSED** | เรียงลำดับจุดแวะตามระยะทางสั้นสุดและซ่อนบ้านเลขที่ก่อนรับงาน |
| `AC-04-02` | `REQ-FN-001`, `BR-03` | `test_AC_04_02_reject_job_exceeding_vehicle_capacity` | PASSED | **PASSED** | บล็อกการรับงานเมื่อน้ำหนักรวมเกิน 200.0 กก. |
| `AC-04-03` | `REQ-FN-003`, `MD-STM-04` | `test_AC_04_03_transition_enroute_to_arrived_weighing` | PASSED | **PASSED** | เปลี่ยนสถานะ `S3 -> S4` พร้อมบันทึกพิกัดและเวลาถึงหน้างาน |
| `AC-04-04` | `REQ-FN-002`, `BR-02` | `test_AC_04_04_weighing_within_15pct_settles_immediately` | PASSED | **PASSED** | ส่วนต่าง +10% คำนวณยอดและเปลี่ยนเป็น `S6` ทันที |
| `AC-04-05` | `REQ-FN-002`, `BR-02` | `test_AC_04_05_weighing_exact_15pct_boundary_passes` | **FAILED** | **PASSED** | รอบแรกตกเพราะ AI ใช้ `>= 0.15` แทน `> 0.15` (`GAP-01`) แก้ในโค้ดโดยไม่แก้ test |
| `AC-04-06` | `REQ-FN-002`, `REQ-FN-004`, `BR-02` | `test_AC_04_06_weighing_over_15pct_triggers_requote_s5` | PASSED | **PASSED** | ส่วนต่าง -20% เปลี่ยนสถานะเป็น `S5` รอผู้ขายยืนยัน |
| `AC-04-07` | `REQ-FN-004`, `MD-STM-04` | `test_AC_04_07_seller_accepts_requote_transitions_to_s6` | PASSED | **PASSED** | ผู้ขายกดยืนยันใน `S5` เปลี่ยนเป็น `S6` และออกใบเสร็จ |
| `AC-04-08` | `REQ-FN-004`, `BR-01` | `test_AC_04_08_seller_rejects_requote_locks_evidence_s8` | PASSED | **PASSED** | ผู้ขายปฏิเสธใน `S5` เปลี่ยนเป็น `S8 (Disputed)` และล็อกภาพตาชั่ง |
| `AC-04-09` | `REQ-OP-001`, `REQ-DAT-001` | `test_AC_04_09_offline_queue_sync_idempotency` | **FAILED** | **PASSED** | รอบแรกตกเพราะส่งซ้ำแล้วเกิดใบเสร็จ 2 ใบ (`GAP-02`) แก้ด้วยการตรวจ `idempotency_key` |
| `AC-04-10` | `REQ-OP-002`, `REQ-QA-001` | `test_AC_04_10_external_map_timeout_haversine_fallback` | PASSED | **PASSED** | เมื่อ Maps API ตอบ 503 สลับใช้ Haversine ภายใน `< 2.0s` (`0.08s`) |
| `AC-04-11` | `REQ-SEC-001`, `REQ-SEC-002` | `test_AC_04_11_pdpa_address_and_phone_masking` | PASSED | **PASSED** | ซ่อนบ้านเลขที่จริงและแปลงเบอร์โทรเป็น `081-XXX-4567` |
| `AC-04-12` | `REQ-BR-002`, `BR-01` | `test_AC_04_12_price_snapshot_locked_at_booking` | PASSED | **PASSED** | ใช้ราคาล็อก ณ เวลานัดหมาย (12.00 บาท/กก.) แม้ราคากลางเปลี่ยนภายหลัง |
