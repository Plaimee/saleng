---
name: tasks
description: แตกแผน plan.md ออกเป็นงานย่อย tasks.md เรียงตามลำดับ พร้อมตารางตรวจความครบของ AC และ Constraints
---

# คำสั่ง `/tasks` (agent-pack v2 สำหรับ Antigravity)

เมื่อผู้ใช้พิมพ์ `/tasks` หรือเรียกคำสั่ง tasks ตามด้วย path เช่น `/tasks specs/002-route/plan.md`:

## ขั้นตอนการทำงาน
1. ปฏิบัติตามกติกาทั้ง 10 ข้อใน [`AGENTS.md`](file:///Users/thirtyzero1/Saleng/AGENTS.md) (**ห้ามอ่าน** `docs/hand-written/`)
2. อ่าน `spec.md`, `rules.md`, `models.md`, `mockups/`, `plan.md` และ [`specs/000-shared/constraints.md`](file:///Users/thirtyzero1/Saleng/specs/000-shared/constraints.md)
3. แตกงานย่อย `T-01`, `T-02`, ... โดยแต่ละงานต้องมีครบ **6 ช่อง** (ทำอะไร, สร้างหรือแก้ไฟล์ไหน, ต้องทำหลัง, รองรับ REQ/AC, Definition of Done, ยังติดคำถามเปิดหรือไม่)
4. ทำตารางตรวจทาน 2 ตารางท้ายไฟล์: ตารางความครบของ AC ทุกข้อ และตารางความครบของ Constraints ทุกข้อ
5. บันทึกผลลง [`prompt-log.md`](file:///Users/thirtyzero1/Saleng/prompt-log.md)
