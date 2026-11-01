# ตารางตามรอยความต้องการสองทิศทาง (`rtm.md`) — ฟีเจอร์ `UC-04` การจัดการงานและเส้นทางของซาเล้ง

เอกสารนี้ไล่สายตามรอยทั้ง **ไปข้างหน้า (Forward Traceability: `EV -> REQ -> BR/MD -> AC -> Test -> Task -> Code/UI`)** และ **ย้อนกลับ (Backward Traceability: `Code/UI -> REQ`)** เพื่อหาสายที่ขาดหรือโค้ดส่วนเกิน

---

## 1. ตารางตามรอยไปข้างหน้า (Forward Traceability Matrix — 16 ความต้องการหลัก)

| รหัส REQ | หลักฐานที่มา (`EV`) | กฎธุรกิจ / โมเดล (`BR` / `MD`) | เกณฑ์การยอมรับ (`AC`) | ชื่อฟังก์ชันทดสอบ (`Test`) | งานย่อย (`Task`) | ไฟล์โค้ด / หน้าจอ (`Code / UI`) | สถานะสายตามรอย |
|---|---|---|---|---|---|---|---|
| `REQ-FN-001` | `EV-01`, `EV-02` | `BR-03`, `MD-DOM-04` | `AC-04-01`, `AC-04-02` | `test_AC_04_01_route_optimization_and_masked_feed`, `test_AC_04_02_reject_job_exceeding_vehicle_capacity` | `T-03`, `T-04`, `T-12` | `backend/app/services/route_service.py`, `mockups/UI-04-01-job-feed.html` | ครบทุกจุด |
| `REQ-FN-002` | `EV-03`, `EV-04` | `BR-01`, `BR-02`, `MD-ACT-04` | `AC-04-04`, `AC-04-05`, `AC-04-06` | `test_AC_04_04_weighing_within_15pct_settles_immediately`, `test_AC_04_05_weighing_exact_15pct_boundary_passes`, `test_AC_04_06_weighing_over_15pct_triggers_requote_s5` | `T-05`, `T-12` | `backend/app/services/weighing_service.py`, `mockups/UI-04-03-reweigh-receipt.html` | ครบทุกจุด |
| `REQ-FN-003` | `EV-02` | `MD-STM-04` (`S1..S9`) | `AC-04-01`, `AC-04-03` | `test_AC_04_03_transition_enroute_to_arrived_weighing` | `T-02`, `T-04`, `T-12` | `backend/app/domain/job_state_machine.py`, `mockups/UI-04-02-route-map.html` | ครบทุกจุด |
| `REQ-FN-004` | `EV-04`, `EV-05` | `BR-02`, `MD-STM-04` (`S5`) | `AC-04-06`, `AC-04-07`, `AC-04-08` | `test_AC_04_07_seller_accepts_requote_transitions_to_s6`, `test_AC_04_08_seller_rejects_requote_locks_evidence_s8` | `T-06`, `T-12` | `backend/app/services/requote_service.py`, `mockups/UI-04-03-reweigh-receipt.html` | ครบทุกจุด |
| `REQ-FN-005` | `EV-05`, `EV-06` | `BR-01`, `MD-DOM-04` | `AC-04-04`, `AC-04-07`, `AC-04-09` | `test_AC_04_04_weighing_within_15pct_settles_immediately`, `test_AC_04_09_offline_queue_sync_idempotency` | `T-07`, `T-08` | `backend/app/services/receipt_service.py`, `mockups/UI-04-03-reweigh-receipt.html` | ครบทุกจุด |
| `REQ-FN-006` | `EV-04`, `EV-08` | `BR-01`, `MD-STM-04` (`S8`) | `AC-04-08` | `test_AC_04_08_seller_rejects_requote_locks_evidence_s8` | `T-06`, `T-09` | `backend/app/services/dispute_service.py` | ครบทุกจุด |
| `REQ-DAT-001` | `EV-03`, `EV-06` | `BR-01`, `MD-DOM-04` | `AC-04-08`, `AC-04-09` | `test_AC_04_08_seller_rejects_requote_locks_evidence_s8`, `test_AC_04_09_offline_queue_sync_idempotency` | `T-02`, `T-07` | `backend/app/models/schema.py` | ครบทุกจุด |
| `REQ-OP-001` | `EV-01`, `EV-07` | `MD-SEQ-04` | `AC-04-09` | `test_AC_04_09_offline_queue_sync_idempotency` | `T-08`, `T-11` | `backend/app/services/offline_sync_service.py` | ครบทุกจุด |
| `REQ-OP-002` | `EV-02`, `EV-07` | `MD-SEQ-04` | `AC-04-10` | `test_AC_04_10_external_map_timeout_haversine_fallback` | `T-04`, `T-11` | `backend/app/adapters/maps_adapter.py` | ครบทุกจุด |
| `REQ-SEC-001` | `EV-08`, `EV-09` | `BR-01`, `MD-DOM-04` | `AC-04-08`, `AC-04-11` | `test_AC_04_08_seller_rejects_requote_locks_evidence_s8` | `T-09`, `T-10` | `backend/app/services/audit_log_service.py` | ครบทุกจุด |
| `REQ-SEC-002` | `EV-08`, `EV-09` | `MD-DOM-04` | `AC-04-01`, `AC-04-11` | `test_AC_04_11_pdpa_address_and_phone_masking` | `T-10`, `T-11` | `backend/app/services/pdpa_masking_service.py`, `mockups/UI-04-01-job-feed.html` | ครบทุกจุด |
| `REQ-QA-001` | `EV-02`, `EV-07` | `MD-SEQ-04` | `AC-04-10` | `test_AC_04_10_external_map_timeout_haversine_fallback` | `T-04`, `T-11` | `backend/app/adapters/maps_adapter.py` | ครบทุกจุด |
| `REQ-QA-002` | `EV-01`, `EV-07` | `MD-SEQ-04` | `AC-04-09` | `test_AC_04_09_offline_queue_sync_idempotency` | `T-08`, `T-11` | `backend/app/services/offline_sync_service.py` | ครบทุกจุด |
| `REQ-UX-001` | `EV-01` | - | `AC-04-01` | `test_AC_04_01_route_optimization_and_masked_feed` + ใบตรวจ `ui-review.md` | `T-12`, `T-13` | `mockups/UI-04-01..03` (ปุ่มขนาด `>= 48px`) | ครบทุกจุด (หลังแก้ `F-02`) |
| `REQ-UX-002` | `EV-01`, `EV-03` | `BR-03` | `AC-04-01`, `AC-04-06` | `test_AC_04_06_weighing_over_15pct_triggers_requote_s5` + ใบตรวจ `ui-review.md` | `T-12`, `T-13` | `mockups/UI-04-03-reweigh-receipt.html` | ครบทุกจุด |
| `REQ-BR-002` | `EV-04`, `EV-06` | `BR-01` | `AC-04-12` | `test_AC_04_12_price_snapshot_locked_at_booking` | `T-03`, `T-13` | `backend/app/services/pricing_snapshot_service.py` | ครบทุกจุด (หลังแก้ `F-01` ด้วย `CR-02`) |

---

## 2. ตารางตามรอยย้อนกลับ (Backward Traceability Matrix — จากไฟล์โค้ดและหน้าจอกลับหา `REQ`)

| โมดูล / ไฟล์โค้ด / หน้าจอ | ฟังก์ชันหรือองค์ประกอบหลัก | อ้างอิง `REQ-ID` | อ้างอิง `AC-ID` | พบโค้ดส่วนเกิน (Orphan Code) หรือไม่? |
|---|---|---|---|---|
| `backend/app/domain/job_state_machine.py` | `transition_state(job, target_state)` (`S1..S9`) | `REQ-FN-003`, `REQ-FN-004` | `AC-04-03`, `AC-04-06`, `AC-04-07`, `AC-04-08` | ไม่พบ |
| `backend/app/services/route_service.py` | `optimize_route()`, `check_vehicle_capacity()` | `REQ-FN-001`, `BR-03` | `AC-04-01`, `AC-04-02` | ไม่พบ |
| `backend/app/adapters/maps_adapter.py` | `calculate_distance_with_fallback()` (`Haversine * 1.35`) | `REQ-OP-002`, `REQ-QA-001` | `AC-04-10` | ไม่พบ |
| `backend/app/services/weighing_service.py` | `record_actual_weight()`, `evaluate_weight_variance()` | `REQ-FN-002`, `BR-02` | `AC-04-04`, `AC-04-05`, `AC-04-06` | ไม่พบ |
| `backend/app/services/pricing_snapshot_service.py` | `snapshot_price_at_booking()` | `REQ-BR-002`, `BR-01` | `AC-04-12` | ไม่พบ |
| `backend/app/services/offline_sync_service.py` | `enqueue_offline_tx()`, `sync_offline_receipt()` | `REQ-OP-001`, `REQ-QA-002`, `REQ-DAT-001` | `AC-04-09` | ไม่พบ |
| `backend/app/services/pdpa_masking_service.py` | `mask_seller_pii()`, `append_audit_log()` | `REQ-SEC-001`, `REQ-SEC-002` | `AC-04-08`, `AC-04-11` | ไม่พบ |
| `specs/002-route/mockups/UI-04-01-job-feed.html` | หน้าจอรายการคิวงานและแถบความจุรถซาเล้ง | `REQ-FN-001`, `REQ-SEC-002`, `REQ-UX-001` | `AC-04-01`, `AC-04-02`, `AC-04-11` | ไม่พบ |
| `specs/002-route/mockups/UI-04-02-route-map.html` | หน้าจอแผนที่นำทางและป้ายแจ้งเตือน Haversine Fallback | `REQ-FN-003`, `REQ-OP-002`, `REQ-QA-001` | `AC-04-01`, `AC-04-03`, `AC-04-10` | ไม่พบ |
| `specs/002-route/mockups/UI-04-03-reweigh-receipt.html` | หน้าจอบันทึกน้ำหนักชั่งจริง แจ้งเตือน ±15% และใบเสร็จดิจิทัล | `REQ-FN-002`, `REQ-FN-004`, `REQ-FN-005`, `REQ-OP-001`, `REQ-BR-002` | `AC-04-04..09`, `AC-04-12` | ไม่พบ (ส่วน `Q-04` อยู่ในกรอบ `.todo`) |

---

## 3. ตารางตรวจสเปก 7 ข้อ และข้อค้นพบ (`F-01` ถึง `F-04`)

| รหัสข้อค้นพบ | ประเภทสายที่ขาด | รายละเอียดที่ตรวจพบ | การตัดสินใจของทีม ("ทีมตัดสิน") |
|---|---|---|---|
| `F-01` | `REQ` มีโค้ดรองรับแต่ขาด `AC` เฉพาะเจาะจงในรอบแรก | `REQ-BR-002` (การล็อกราคากลาง ณ เวลานัดหมาย `Price Snapshot`) มีฟังก์ชัน `snapshot_price_at_booking()` แล้ว แต่ในรอบแรกยังไม่มี Scenario ทดสอบกรณีแอดมินแก้ราคากลางระหว่างซาเล้งกำลังเดินทาง | **ทีมตัดสิน:** ออกใบขอเปลี่ยนแปลง `CR-02` เพิ่ม `@AC-04-12` และ `test_AC_04_12_price_snapshot_locked_at_booking` (ดำเนินการเสร็จสิ้น) |
| `F-02` | `REQ` เชิง UX มีระบุในสเปกแต่ขาดข้อตรวจในใบตรวจหน้าจอ | `REQ-UX-001` กำหนดขนาดปุ่มกดไม่น้อยกว่า `48x48 px` สำหรับซาเล้งกดมือเดียวกลางแจ้ง ซึ่ง `pytest` หลังบ้านตรวจไม่ได้ | **ทีมตัดสิน:** เพิ่มเป็นข้อตรวจบังคับใน `ui-review/ui-review.md` และปรับปรุง CSS ใน `GAP-05` |
| `F-03` | คำถามเปิด `Q-04` เสี่ยงถูกสร้างเป็นโค้ดจริง | ใน `UI-04-03` มีแนวคิดเรื่องค่าธรรมเนียมเดินทางหากผู้ขายปฏิเสธหลังชั่งจริง ซึ่งยังไม่ได้ข้อสรุป (`Q-04` / `AS-01`) | **ทีมตัดสิน:** ตีกรอบเส้นประ `.todo` ใน `UI-04-03-reweigh-receipt.html` และห้ามสร้างปุ่มหักเงินในโค้ดจริงจนกว่าจะอนุมัติในเฟสถัดไป |
| `F-04` | ตรวจหา Orphan Code (โค้ดที่ไม่มี `REQ` รองรับ) | ไล่ตรวจทุกฟังก์ชันในบริการหลังบ้านและองค์ประกอบหน้าจอเทียบกับตาราง `REQ` | **ทีมตัดสิน:** ไม่พบโค้ดส่วนเกิน ทุกฟังก์ชันผูกกับ `REQ` ของกลุ่ม 2 ครบถ้วน |
