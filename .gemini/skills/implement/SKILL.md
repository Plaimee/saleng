---
name: implement
description: ลงมือเขียนโค้ดและ Automated Test ทีละ Task ตาม tasks.md พร้อมตรวจ 5 ด่านก่อนจบงาน
---

# คำสั่ง `/implement` (agent-pack v2 สำหรับ Antigravity)

เมื่อผู้ใช้พิมพ์ `/implement` หรือเรียกคำสั่ง implement ตามด้วยรหัสงาน เช่น `/implement T-01`:

## ขั้นตอนการทำงาน
1. ปฏิบัติตามกติกาทั้ง 10 ข้อใน [`AGENTS.md`](file:///Users/thirtyzero1/Saleng/AGENTS.md)
2. รับรหัสงานและทำทีละ 1 Task เท่านั้น ห้ามรวบหลายงาน
3. แตะเฉพาะไฟล์ที่ระบุในช่อง **"สร้างหรือแก้ไฟล์ไหน"** ของ Task นั้น
4. เขียนโค้ดและ Automated Test ให้ชื่อฟังก์ชันตรงกับรหัส AC เช่น `test_AC_04_01`
5. ตรวจ 5 ด่านหลังจบงาน (ตรวจ `git status`, ผลรัน test, การอ้างอิง ID, Non-goals/คำถามเปิด, บันทึกลง `tasks.md`, `prompt-log.md` และ `gaps.md`)
