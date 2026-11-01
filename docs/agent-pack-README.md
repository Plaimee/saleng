# คู่มือการใช้งานชุดคำสั่ง `agent-pack` รุ่น 2 (`docs/agent-pack-README.md`)

ชุดคำสั่ง `agent-pack` รุ่น 2 สำหรับวิชา **517 511 Requirements Engineering** ออกแบบมาเพื่อทำงานตามแนวทาง **Spec-Driven Development (SDD)** ด้วย 4 คำสั่งหลัก (`/clarify`, `/plan`, `/tasks`, `/implement`) โดยรองรับทั้ง GitHub Copilot, Claude Code และ Cursor

---

## 1. โครงสร้างไฟล์ของชุดคำสั่ง

| ตำแหน่งไฟล์ | เครื่องมือที่อ่าน | หน้าที่ |
|---|---|---|
| [`AGENTS.md`](../AGENTS.md) | Copilot, Cursor, และ AI Agent ทุกตัว | กติกากลาง 10 ข้อ (รวมรหัสของวิชา ข้อจำกัดกลาง และกฎการอ่าน Mockup) |
| [`CLAUDE.md`](../CLAUDE.md) | Claude Code | ชี้ให้ Claude Code เปิดอ่าน `AGENTS.md` ก่อนทำคำสั่งทุกครั้ง |
| [`.github/prompts/*.prompt.md`](../.github/prompts/) | GitHub Copilot Chat (Agent Mode) | คำสั่ง `/clarify`, `/plan`, `/tasks`, `/implement` |
| [`.claude/commands/*.md`](../.claude/commands/) | Claude Code | คำสั่งชุดเดียวกันสำหรับ Claude Code |
| [`.cursor/commands/*.md`](../.cursor/commands/) | Cursor | คำสั่งชุดเดียวกันสำหรับ Cursor |

---

## 2. ลำดับการใช้คำสั่งในรอบแรก (Round 1 — ก่อนตั้ง Baseline `B1`)

1. **`/clarify specs/002-route/spec.md`** — ให้ AI อ่านสเปกและถามคำถามไม่เกิน 8 ข้อ (ห้ามแก้ไฟล์ในรอบถาม) จากนั้นทีมตอบแบบ `"รู้ เพราะ..."` หรือ `"ไม่รู้ ต้องถาม..."` แล้วบันทึกเป็น `spec v2`
2. **`/plan specs/002-route/spec.md`** — ให้ AI สร้างแผนทางเทคนิค `plan.md` 8 ส่วน พร้อมตรวจ Constraints กลางครบทุกข้อ
3. **`/tasks specs/002-route/plan.md`** — ให้ AI แตกงานย่อย `T-01..T-13` ใน `tasks.md` พร้อมตารางตรวจความครบของ `AC` และ `Constraints`
4. **`/implement T-01` .. `/implement T-13`** — สั่งทำทีละ Task ตรวจ 5 ข้อหลังจบแต่ละ Task รันชุดทดสอบจริง บันทึกผลลง `test-run.txt`, `ac-results.md`, และ `gaps.md`

---

## 3. วิธีใช้ในรอบเปลี่ยนแปลง (Round 2 — หลังตั้ง Baseline `B1`)

| ขั้นตอน | สิ่งที่ทำ | ผลลัพธ์ที่ได้ |
|---|---|---|
| **ขั้นที่ 1** | สั่ง AI ร่าง `rtm.md` จากแม่แบบ [`docs/rtm-template.md`](rtm-template.md) แล้วทีมตรวจเองทุกแถว | พบสายที่ขาด (`F-01..F-04`) ใน [`specs/002-route/rtm.md`](../specs/002-route/rtm.md) |
| **ขั้นที่ 2** | วางไฟล์หน้าจอต้นแบบใน `mockups/` ตาม [`docs/mockup-guide.md`](mockup-guide.md) และสั่ง `/clarify` | AI เห็นสถานะ `baselined` จึงไม่แก้ `spec.md` เอง แต่ช่วยร่างใบขอเปลี่ยนแปลง `CR` |
| **ขั้นที่ 3** | เขียนใบขอเปลี่ยนแปลงใน `changes/CR-xx.md` ตามแม่แบบ [`docs/cr-template.md`](cr-template.md) และขออนุมัติ | ได้ `CR-01..04` ที่ผ่านการวิเคราะห์ผลกระทบครบ 6 ส่วน |
| **ขั้นที่ 4** | แยกข้อจำกัดที่ใช้ร่วมกันทุกฟีเจอร์ไว้ที่ [`specs/000-shared/constraints.md`](../specs/000-shared/constraints.md) | ไม่ต้องคัดลอกข้อความซ้ำในทุกฟีเจอร์ (กติกาข้อ 9) |
| **ขั้นที่ 5** | อัปเดต `plan.md` และ `tasks.md` เฉพาะจุดที่กระทบจาก `CR` แล้วสั่ง `/implement` เฉพาะ Task ที่เพิ่ม/แก้ | โค้ดและหน้าจออัปเดตตรงตาม `CR` |
| **ขั้นที่ 6** | รันชุดทดสอบซ้ำทั้งชุด + ตรวจหน้าจอ 6 ข้อ (`ui-review.md`) แล้วประกาศ Baseline `B2` | ผ่านครบทุก AC และตรงกับ Mockup ครบทุกหน้าจอ |
