# ขอบเขตงานและเงื่อนไขการส่งมอบ (`docs/acceptance/tor-r1.md` — TOR Release R1)

เอกสารนี้สรุปข้อกำหนดขอบเขตงาน (Terms of Reference — TOR) และตารางรายการส่งมอบงานงวดที่ 1 (`Release R1`) ของโครงการแพลตฟอร์มรับซื้อของเก่า Saleng (กลุ่ม 2)

---

## 1. รายการสิ่งส่งมอบตามสัญญา (Deliverables Checklist — Release `R1`)

| ลำดับ | รายการสิ่งส่งมอบ (Deliverable) | ตำแหน่งไฟล์ใน Repository | เกณฑ์การตรวจรับ (Acceptance Criteria) | สถานะการส่งมอบ |
|---:|---|---|---|---|
| 1 | **เอกสารข้อกำหนดความต้องการซอฟต์แวร์ (SRS & Baseline `B2`)** | [`docs/srs/`](../srs/) (`problem-statement.md`, `evidence.md`, `catalogue.md`, `UC-04.md`, `decisions.md`, `glossary.md`, `baseline.md`) | มีสายตามรอยตั้งแต่ `PB -> SH -> EV -> REQ -> AC` ครบทั้ง 16 ความต้องการหลัก | ส่งมอบครบ |
| 2 | **สเปกฟีเจอร์หลัก `UC-04` พร้อมโมเดล 6 มุมมองและ Gherkin** | [`specs/002-route/`](../../specs/002-route/) (`spec.md`, `route.feature`, `rules.md`, `models.md`) และสเปกครบ 20 ฟีเจอร์แยกตามบทบาทใน [`specs/`](../../specs/) | ผ่านการตรวจความสอดคล้อง 7 ข้อ และไม่มีคำถามเปิดที่กระทบการทำงานหลักค้างโดยไม่มีสมมติฐานรองรับ | ส่งมอบครบ |
| 3 | **หน้าจอต้นแบบ (HTML Mockups) และผลตรวจหน้าจอ 6 ข้อ** | [`specs/002-route/mockups/`](../../specs/002-route/mockups/) (`UI-04-01..03`) และ [`ui-review.md`](../../specs/002-route/ui-review/ui-review.md) | ตรวจผ่านครบ 18/18 ข้อตรวจ (3 หน้าจอ × 6 ข้อ) | ส่งมอบครบ |
| 4 | **ตารางตามรอยสองทิศทาง (Bidirectional RTM) และบันทึกการเปลี่ยนแปลง (`CR`)** | [`specs/002-route/rtm.md`](../../specs/002-route/rtm.md) และ [`changes/CR-01..04.md`](../../changes/) | ทุก `REQ` มี `AC` และ `Test` รองรับครบ 100% ไม่พบโค้ดส่วนเกิน (Orphan Code) | ส่งมอบครบ |
| 5 | **ผลรันชุดทดสอบอัตโนมัติและสเปกปฏิบัติการ/AI** | [`specs/002-route/test-run.txt`](../../specs/002-route/test-run.txt), [`ac-results.md`](../../specs/002-route/ac-results.md), [`docs/srs/operations.md`](../srs/operations.md), [`eval/qa.jsonl`](../../eval/qa.jsonl) | รันผ่านครบ `12 / 12 AC` และชุดทดสอบ AI 20 เคสครบถ้วน | ส่งมอบครบ |
