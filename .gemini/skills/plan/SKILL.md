---
name: plan
description: สร้างแผนการพัฒนาทางเทคนิค plan.md 8 ส่วนจาก spec.md ที่ผ่านการ clarify แล้ว โดยไม่อ่าน docs/hand-written/
---

# คำสั่ง `/plan` (agent-pack v2 สำหรับ Antigravity)

เมื่อผู้ใช้พิมพ์ `/plan` หรือเรียกคำสั่ง plan ตามด้วย path เช่น `/plan specs/002-route/spec.md`:

## ขั้นตอนการทำงาน
1. ปฏิบัติตามกติกาทั้ง 10 ข้อใน [`AGENTS.md`](file:///Users/thirtyzero1/Saleng/AGENTS.md) (**ห้ามอ่าน** `docs/hand-written/`)
2. อ่าน `spec.md`, `rules.md`, `models.md`, `.feature`, `mockups/` และ [`specs/000-shared/constraints.md`](file:///Users/thirtyzero1/Saleng/specs/000-shared/constraints.md)
3. ตรวจสอบเทคโนโลยี: หากใน `constraints.md` ไม่ได้ระบุเทคโนโลยีไว้ ให้ถามทีมก่อนว่าใช้เทคโนโลยีใด แล้วเขียนกำกับว่า `"ทีมเลือกเอง ไม่ได้มาจาก spec"`
4. สร้างหรืออัปเดตไฟล์ `plan.md` ครบ 8 ส่วน (เทคโนโลยี, โครงสร้างไฟล์, ฐานข้อมูล, API, แผนเชื่อมระบบภายนอก, แผนหน้าจอ, การจัดการคำถามเปิด, ตารางตรวจ Constraints กลาง)
5. บันทึกผลลง [`prompt-log.md`](file:///Users/thirtyzero1/Saleng/prompt-log.md)
