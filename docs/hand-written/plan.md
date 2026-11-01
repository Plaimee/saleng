# แผนทางเทคนิคที่ทีมเขียนเองด้วยมือ (`docs/hand-written/plan.md`) — ก่อนใช้คำสั่ง `/plan`

> **หมายเหตุ:** ไฟล์นี้คือแผนที่ทีมกลุ่ม 2 เขียนเองด้วยมือก่อนสั่ง AI โดย AI ไม่ได้รับอนุญาตให้อ่านโฟลเดอร์ `docs/hand-written/` (ตามกติกาใน `AGENTS.md`) ใช้สำหรับเปรียบเทียบกับแผนของ AI ใน [`specs/002-route/plan-compare.md`](../../specs/002-route/plan-compare.md)

---

## 1. เทคโนโลยีที่ทีมตั้งใจเลือกใช้
- **Backend:** Python (FastAPI)
- **Database:** SQLite เก็บข้อมูลใบงานและใบเสร็จ
- **Frontend:** HTML / JavaScript สำหรับหน้าจอมือถือของซาเล้ง

## 2. โครงสร้างตารางฐานข้อมูล (ร่างโดยทีม)
1. ตาราง `jobs`: เก็บ `id`, `seller_name`, `seller_phone`, `address`, `est_weight`, `actual_weight`, `price_per_kg`, `status` (`S1..S9`)
2. ตาราง `receipts`: เก็บ `id`, `job_id`, `total_amount`, `created_at`

## 3. รายการ API ที่ทีมคิดไว้
- `GET /jobs/nearby` — ดึงใบงานใกล้ตัวซาเล้งในรัศมี 5 กม.
- `POST /jobs/{id}/accept` — ซาเล้งกดรับงานและเช็คว่าน้ำหนักรวมเกิน 200 กก. หรือไม่
- `POST /jobs/{id}/weigh` — บันทึกน้ำหนักชั่งจริง ถ้าต่างจากเดิมเกิน 15% ให้เปลี่ยนสถานะเป็น `S5` รอผู้ขายกดตกลง (ถ้าผู้ขายปฏิเสธให้หักค่าเดินทาง 30 บาทและเปลี่ยนเป็น `S8`)
- `POST /jobs/{id}/receipt` — ออกใบเสร็จรับเงิน (กรณีเน็ตหลุดให้เก็บไว้ใน LocalStorage ของเบราว์เซอร์ก่อน)
