# ทะเบียนการตั้ง Baseline (`docs/srs/baseline.md`) — โครงการ Saleng (กลุ่ม 2)

เอกสารนี้บันทึกการประกาศ Baseline ทั้ง 2 รอบ (`BL-01 / B1` ก่อนเริ่มเขียนโค้ด และ `BL-02 / B2` หลังผ่านการตรวจ RTM, Mockup Review และการจัดการใบขอเปลี่ยนแปลง `CR-01..04`)

---

## 1. ตารางสรุป Baseline (`B1` และ `B2`)

| รหัส Baseline | ชื่อ Baseline | วันที่ประกาศ | ขอบเขตเอกสารและโค้ดที่ถูกล็อก | เงื่อนไขการแก้ไขหลังตั้ง Baseline |
|---|---|---|---|---|
| **`BL-01` (`B1`)** | **Initial Specification Baseline** | 2026-03-07 | - `docs/srs/{problem-statement.md, evidence.md, catalogue.md, UC-04.md, glossary.md}`<br>- `specs/002-route/{spec.md v2.0, route.feature, rules.md, models.md}` | ห้ามแก้ `REQ`, `AC`, `BR`, `MD` โดยตรง หากต้องการเปลี่ยนต้องยื่นใบขอเปลี่ยนแปลง (`CR-xx.md`) ในโฟลเดอร์ `changes/` |
| **`BL-02` (`B2`)** | **Verified Implementation & UI Baseline (Release R1 Candidate)** | 2026-03-14 | - เอกสารใน `B1` ทั้งหมดที่อัปเดตตาม `CR-01..CR-04`<br>- `specs/000-shared/{constraints.md, quality-requirements.md}`<br>- `specs/002-route/{plan.md, tasks.md, rtm.md, ac-results.md, gaps.md, mockups/*, ui-review/ui-review.md}` | พร้อมเข้าสู่การทดสอบตรวจรับ (`UAT`) ตาม [`docs/acceptance/uat-plan.md`](../acceptance/uat-plan.md) |

---

## 2. ประวัติใบขอเปลี่ยนแปลงระหว่าง `B1 -> B2` (`CR-01` ถึง `CR-04`)

| รหัส CR | หัวข้อการเปลี่ยนแปลง | ที่มา | สถานะ | ไฟล์หลักที่แก้ไข |
|---|---|---|---|---|
| [`CR-01`](../../changes/CR-01.md) | ปรับเกณฑ์ส่วนต่างน้ำหนักชั่งจริงให้ชัดเจนที่ `±15%` (`BR-02`) และเพิ่มการตรวจขอบเขต `15.0%` พอดี (`AC-04-05`) | ผลจาก `/clarify` รอบแรก (`Q-01`, `D-01`) | อนุมัติและดำเนินการแล้ว | `specs/002-route/rules.md`, `specs/002-route/route.feature` |
| [`CR-02`](../../changes/CR-02.md) | เพิ่มเกณฑ์การยอมรับ `@AC-04-12` สำหรับการล็อกราคากลาง ณ เวลานัดหมาย (`REQ-BR-002` Price Snapshot) | ข้อค้นพบ `F-01` จากตารางตามรอย `rtm.md` | อนุมัติและดำเนินการแล้ว | `specs/002-route/route.feature`, `specs/002-route/ac-results.md`, `specs/002-route/rtm.md` |
| [`CR-03`](../../changes/CR-03.md) | เพิ่มป้ายเตือนสีส้มเมื่อส่วนต่างน้ำหนักเกิน `±15%` (`S5`) และป้ายสถานะรอซิงก์ออฟไลน์บนหน้าจอ `UI-04-03` | การเดินดู Mockup ร่วมกับ `SH-01` และ `SH-02` | อนุมัติและดำเนินการแล้ว | `specs/002-route/mockups/UI-04-03-reweigh-receipt.html`, `specs/002-route/spec.md` |
| [`CR-04`](../../changes/CR-04.md) | แยกข้อจำกัดกลางของทั้งแพลตฟอร์มไปไว้ที่ `specs/000-shared/constraints.md` ตามกติกาข้อ 9 | การอัปเกรดชุดคำสั่งเป็น `agent-pack v2` | อนุมัติและดำเนินการแล้ว | `specs/000-shared/constraints.md`, `specs/002-route/plan.md` |
