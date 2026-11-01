# โครงการ Saleng (แพลตฟอร์มรับซื้อของเก่า/ซาเล้ง — กลุ่ม 2)

คลังสเปกและเอกสารวิศวกรรมความต้องการ (Requirements Engineering & Spec-Driven Development) ของ **กลุ่ม 2** จัดโครงสร้างตามแม่แบบมาตรฐานของรายวิชา **`reqeng-template` (`agent-pack v2`)** ครบทุกโฟลเดอร์ พร้อมสเปกแยกตามบทบาทครบทั้ง 20 ฟีเจอร์ (`ผู้ขาย 9` + `ผู้ซื้อ/ซาเล้ง 7` + `ผู้ดูแลระบบ 4`)

---

## 1. โครงสร้าง Repository (เทียบเคียง `ppsajja/reqeng-template` รุ่น 2)

```text
Saleng/
├── AGENTS.md                        # กติกากลาง 10 ข้อสำหรับ AI Coding Agent (agent-pack v2)
├── CLAUDE.md                        # ไฟล์ชี้ไปที่ AGENTS.md สำหรับ Claude Code
├── prompt-log.md                    # บันทึกการใช้คำสั่ง /clarify /plan /tasks /implement ทุกครั้ง
├── .github/prompts/                 # คำสั่ง /clarify /plan /tasks /implement สำหรับ GitHub Copilot
├── .claude/commands/                # คำสั่งชุดเดียวกันสำหรับ Claude Code
├── .cursor/commands/                # คำสั่งชุดเดียวกันสำหรับ Cursor
├── specs/                           # โฟลเดอร์สเปกชั้นนอกสุด (AI อ่านจากที่นี่เสมอ)
│   ├── 000-shared/                  # ข้อจำกัดกลาง (constraints.md) และความต้องการเชิงคุณภาพ (quality-requirements.md)
│   ├── 002-route/                   # ฟีเจอร์หลัก UC-04 ตามโครงสร้างมาตรฐาน specs/002-<feature>/ ของ reqeng-template
│   │   ├── spec.md                  # สเปกหลัก (11 ส่วน + ตารางเชื่อมโยง UI Mockups)
│   │   ├── route.feature            # เกณฑ์การยอมรับ Gherkin (@AC-04-01 ถึง @AC-04-12)
│   │   ├── rules.md                 # กฎธุรกิจ BR-01..03 และตารางการตัดสินใจ (Decision Table)
│   │   ├── models.md                # โมเดล Mermaid 6 มุมมอง (CTX, UCD, ACT, STM S1..S9, DOM, SEQ)
│   │   ├── plan.md                  # แผนทางเทคนิค 8 ส่วนที่สร้างจาก /plan
│   │   ├── tasks.md                 # งานย่อย T-01..T-13 (6 ช่องต่อ Task + ตารางตรวจ AC และ Constraints)
│   │   ├── plan-compare.md          # ตารางเปรียบเทียบแผนที่เขียนเอง vs แผนจาก AI
│   │   ├── tasks-compare.md         # ตารางเปรียบเทียบงานที่แตกเอง vs งานจาก AI
│   │   ├── test-run.txt             # ผลการรัน pytest (12 passed)
│   │   ├── ac-results.md            # ตารางสรุปผล AC รอบที่ 1 และรอบที่ 2
│   │   ├── gaps.md                  # บันทึกช่องว่าง GAP-01..05 พร้อมจำแนกสาเหตุ (spec | ai | test | env)
│   │   ├── rtm.md                   # ตารางตามรอยสองทิศทาง (Forward / Backward / ข้อค้นพบ F-01..04)
│   │   ├── changes/CR-03.md         # ใบขอเปลี่ยนแปลงระดับฟีเจอร์
│   │   ├── mockups/                 # หน้าจอ HTML ต้นแบบ (UI-04-01, UI-04-02, UI-04-03)
│   │   └── ui-review/ui-review.md   # ใบตรวจหน้าจอเทียบกับ Mockup 6 ข้อ (18/18 ข้อตรวจ)
│   ├── SPEC-ROUTE/                  # โฟลเดอร์คู่ขนานของ UC-04 (ซิงก์ไฟล์ตรงกับ 002-route ทุกประการ)
│   ├── seller/                      # สเปกฝั่งผู้ขาย 9 ฟีเจอร์ (01 ถึง 09)
│   ├── buyer/                       # สเปกฝั่งผู้ซื้อ/ซาเล้ง 7 ฟีเจอร์ (01 ถึง 07)
│   └── admin/                       # สเปกฝั่งผู้ดูแลระบบ 4 ฟีเจอร์ (01 ถึง 04)
├── docs/
│   ├── agent-pack-README.md         # คู่มือการใช้คำสั่ง agent-pack รุ่น 2
│   ├── shared-constraints-guide.md  # คู่มือการใช้ไฟล์ Constraint กลาง (กติกาข้อ 9)
│   ├── mockup-guide.md              # คู่มือการทำ HTML Mockup และตรวจ 6 ข้อ (กติกาข้อ 10)
│   ├── mockup-template.html         # แม่แบบไฟล์ HTML Mockup
│   ├── cr-template.md               # แม่แบบใบขอเปลี่ยนแปลง 6 ส่วน
│   ├── rtm-template.md              # แม่แบบตารางตามรอย 5 ส่วน
│   ├── constraints-template.md      # แม่แบบไฟล์ข้อจำกัดกลาง
│   ├── srs/                         # เอกสาร SRS ต้นทางของกลุ่ม 2 (Problem, Evidence, Catalogue, UC, Glossary, Baseline, ORS, AI Spec)
│   ├── hand-written/                # แผน plan.md และ tasks.md ที่ทีมเขียนเองด้วยมือ (AI ไม่อ่าน)
│   └── acceptance/                  # แผน UAT, ใบตรวจรับ Release R1, TOR และตารางระดับความรุนแรง S1..S4
├── changes/                         # ใบขอเปลี่ยนแปลงหลังตั้ง Baseline (CR-01 ถึง CR-04)
└── eval/                            # ชุดข้อมูลทดสอบ AI และกฎธุรกิจ 20 เคส (eval/qa.jsonl)
```

---

## 2. ลิงก์ด่วนไปยังเอกสารสำคัญตามบทเรียน (Studios 1–13)

### 2.1 ชุดคำสั่งและกติกาควบคุม AI (`agent-pack v2`)
- กติกากลาง 10 ข้อ: [`AGENTS.md`](AGENTS.md) และ [`CLAUDE.md`](CLAUDE.md)
- บันทึกการใช้คำสั่ง AI: [`prompt-log.md`](prompt-log.md)
- คำสั่ง Slash Commands: [`.github/prompts/`](.github/prompts/), [`.claude/commands/`](.claude/commands/), [`.cursor/commands/`](.cursor/commands/)
- คู่มือและแม่แบบจาก `reqeng-template`: [`docs/agent-pack-README.md`](docs/agent-pack-README.md), [`docs/shared-constraints-guide.md`](docs/shared-constraints-guide.md), [`docs/mockup-guide.md`](docs/mockup-guide.md), [`docs/cr-template.md`](docs/cr-template.md), [`docs/rtm-template.md`](docs/rtm-template.md)

### 2.2 เอกสาร SRS ต้นทาง (`docs/srs/`)
- นิยามปัญหา (`PB-01..04`) และผู้มีส่วนได้เสีย (`SH-01..08`): [`docs/srs/problem-statement.md`](docs/srs/problem-statement.md)
- ทะเบียนหลักฐาน (`EV-01..09`): [`docs/srs/evidence.md`](docs/srs/evidence.md)
- บัญชีรายการความต้องการ 16 ข้อ (`Catalogue`): [`docs/srs/catalogue.md`](docs/srs/catalogue.md)
- สารบัญ Use Cases (`UC-01..13`) และรายละเอียด `UC-04`: [`docs/srs/use-cases.md`](docs/srs/use-cases.md), [`docs/srs/UC-04.md`](docs/srs/UC-04.md)
- บันทึกการเจรจาต่อรองและการตัดสินใจ (`DR-01..02`, `D-01..03`): [`docs/srs/decisions.md`](docs/srs/decisions.md)
- อภิธานศัพท์โดเมนและสถานะใบงาน `S1..S9`: [`docs/srs/glossary.md`](docs/srs/glossary.md)
- ทะเบียนการตั้ง Baseline (`B1` และ `B2`): [`docs/srs/baseline.md`](docs/srs/baseline.md)
- สเปกปฏิบัติการ ความมั่นคง และ PDPA (`ORS`): [`docs/srs/operations.md`](docs/srs/operations.md)
- สเปกขอบเขต AI และการประเมินผล 20 เคส: [`docs/srs/ai-spec.md`](docs/srs/ai-spec.md), [`eval/qa.jsonl`](eval/qa.jsonl)

### 2.3 แพ็กเกจ SDD ฟีเจอร์หลัก `UC-04` (`specs/002-route/` และ `specs/SPEC-ROUTE/`)
- สเปก กฎธุรกิจ โมเดล และ Gherkin: [`spec.md`](specs/002-route/spec.md), [`route.feature`](specs/002-route/route.feature), [`rules.md`](specs/002-route/rules.md), [`models.md`](specs/002-route/models.md)
- แผนและงานย่อย (เทียบกับฉบับเขียนมือ): [`plan.md`](specs/002-route/plan.md), [`tasks.md`](specs/002-route/tasks.md), [`plan-compare.md`](specs/002-route/plan-compare.md), [`tasks-compare.md`](specs/002-route/tasks-compare.md), [`docs/hand-written/plan.md`](docs/hand-written/plan.md), [`docs/hand-written/tasks.md`](docs/hand-written/tasks.md)
- ผลการทดสอบ ช่องว่าง และตารางตามรอย: [`test-run.txt`](specs/002-route/test-run.txt), [`ac-results.md`](specs/002-route/ac-results.md), [`gaps.md`](specs/002-route/gaps.md), [`rtm.md`](specs/002-route/rtm.md)
- หน้าจอต้นแบบและการตรวจ 6 ข้อ: [`UI-04-01-job-feed.html`](specs/002-route/mockups/UI-04-01-job-feed.html), [`UI-04-02-route-map.html`](specs/002-route/mockups/UI-04-02-route-map.html), [`UI-04-03-reweigh-receipt.html`](specs/002-route/mockups/UI-04-03-reweigh-receipt.html), [`ui-review.md`](specs/002-route/ui-review/ui-review.md)
- ใบขอเปลี่ยนแปลง (`CR-01..04`) และการตรวจรับ (`UAT`): [`changes/CR-01.md`](changes/CR-01.md), [`changes/CR-02.md`](changes/CR-02.md), [`changes/CR-03.md`](changes/CR-03.md), [`changes/CR-04.md`](changes/CR-04.md), [`docs/acceptance/uat-plan.md`](docs/acceptance/uat-plan.md), [`docs/acceptance/acceptance-record-r1.md`](docs/acceptance/acceptance-record-r1.md), [`docs/acceptance/tor-r1.md`](docs/acceptance/tor-r1.md), [`docs/acceptance/severity-matrix.md`](docs/acceptance/severity-matrix.md)

### 2.4 สเปกแยกตามบทบาทครบ 20 ฟีเจอร์
- **ฝั่งผู้ขาย (9 ฟีเจอร์):** [`01-account-and-access`](specs/seller/01-account-and-access/spec.md) | [`02-scrap-posting`](specs/seller/02-scrap-posting/spec.md) | [`03-buyer-search-and-selection`](specs/seller/03-buyer-search-and-selection/spec.md) | [`04-pricing-and-decision`](specs/seller/04-pricing-and-decision/spec.md) | [`05-booking-and-status-tracking`](specs/seller/05-booking-and-status-tracking/spec.md) | [`06-payment-and-verification`](specs/seller/06-payment-and-verification/spec.md) | [`07-post-transaction`](specs/seller/07-post-transaction/spec.md) | [`08-notifications-and-help`](specs/seller/08-notifications-and-help/spec.md) | [`09-transaction-safety`](specs/seller/09-transaction-safety/spec.md)
- **ฝั่งผู้ซื้อ/ซาเล้ง (7 ฟีเจอร์):** [`01-account-and-kyc`](specs/buyer/01-account-and-kyc/spec.md) | [`02-job-search-and-map`](specs/buyer/02-job-search-and-map/spec.md) | [`03-pre-acceptance-evaluation`](specs/buyer/03-pre-acceptance-evaluation/spec.md) | [`04-job-and-route-management`](specs/buyer/04-job-and-route-management/spec.md) | [`05-communication`](specs/buyer/05-communication/spec.md) | [`06-billing-and-receipt`](specs/buyer/06-billing-and-receipt/spec.md) | [`07-post-job`](specs/buyer/07-post-job/spec.md)
- **ฝั่งผู้ดูแลระบบ (4 ฟีเจอร์):** [`01-user-verification-and-penalty`](specs/admin/01-user-verification-and-penalty/spec.md) | [`02-benchmark-price-and-grading`](specs/admin/02-benchmark-price-and-grading/spec.md) | [`03-dispatch-and-dispute-center`](specs/admin/03-dispatch-and-dispute-center/spec.md) | [`04-audit-security-and-compliance`](specs/admin/04-audit-security-and-compliance/spec.md)
