---
name: clarify
description: อ่าน spec.md, rules.md, models.md, .feature, mockups/ และ constraints.md เพื่อค้นหาจุดกำกวม ถามกลับก่อนเริ่มทำแผน
---

# คำสั่ง `/clarify` (agent-pack v2 สำหรับ Antigravity)

เมื่อผู้ใช้พิมพ์ `/clarify` หรือเรียกคำสั่ง clarify ตามด้วย path ของ spec เช่น `/clarify specs/002-route/spec.md`:

## ขั้นตอนการทำงาน
1. เปิดอ่านและปฏิบัติตามกติกาทั้ง 10 ข้อใน [`AGENTS.md`](file:///Users/thirtyzero1/Saleng/AGENTS.md)
2. อ่านไฟล์ `spec.md` เป้าหมาย และไฟล์ประกอบในโฟลเดอร์เดียวกัน (`route.feature`, `rules.md`, `models.md`, `mockups/`) รวมถึง [`specs/000-shared/constraints.md`](file:///Users/thirtyzero1/Saleng/specs/000-shared/constraints.md)
3. ตรวจสถานะ `Status:` ที่หัวไฟล์ `spec.md`:
   - ถ้ายัง `draft`: ตอบคำถามเสร็จสามารถอัปเดตสเปกได้
   - ถ้า `baselined`: **ห้ามแก้สเปกตรง ๆ** ให้ร่างใบขอเปลี่ยนแปลง `CR-xx.md` ใน `changes/` ให้ทีมอนุมัติก่อน
4. **ตอบกลับ 3 ส่วน (ห้ามแก้ไฟล์ใด ๆ ในรอบแรก):**
   - **ส่วน A:** จุดที่กำกวม / ไม่ครบ / ขัดกัน
   - **ส่วน B:** คำถามไม่เกิน 8 ข้อ (ระบุ ID, สิ่งที่ต้องเดาถ้าไม่ตอบ, และ SH ที่ตอบได้)
   - **ส่วน C:** สิ่งที่จะต้องเดาถ้าเริ่มโค้ดตอนนี้
5. เมื่อทีมตอบคำถามแบบ *"รู้ เพราะ..."* หรือ *"ไม่รู้ ต้องถาม..."* ให้นำคำตอบไปบันทึกเป็น `D-xx` หรือ `Q-xx`/`AS-xx` และจดลง [`prompt-log.md`](file:///Users/thirtyzero1/Saleng/prompt-log.md)
