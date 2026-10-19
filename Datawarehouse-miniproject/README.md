# 3Eyes Industrial Fastener Anomaly Detection
## Business Intelligence & Quality Control Storytelling System

[![Project Status: Ready](https://img.shields.io/badge/Status-Complete-success.svg)](#)
[![Domain: Automotive Fasteners](https://img.shields.io/badge/Industry-Automotive%20Manufacturing-blue.svg)](#)
[![Subject: Business Idea Creation](https://img.shields.io/badge/Course-Business%20Idea%20Creation-purple.svg)](#)

---

## 1. Executive Summary & Problem Statement

ในอุตสาหกรรมผลิตชิ้นส่วนยานยนต์ (Automotive Fasteners) น็อตและสกรูถือเป็น **Safety-Critical Components** ที่ผิดพลาดไม่ได้ หากมีตำหนิหลุดรอดไปถึงโรงประกอบยานยนต์ จะนำไปสู่การเบรกล็อตประกอบ (Line-stop Penalty) และการเคลมสินค้ามูลค่ามหาศาล

### Pain Points เดิม (Before 3Eyes)
1. **Defect Escape Rate สูง (1,500 PPM):** การใช้สายตามนุษย์ส่องกล้องตรวจ (Manual Visual Inspection) เกิดความล้า (Fatigue) โดยเฉพาะเกลียวด้านข้างและรอยร้าวใต้หัวน็อต
2. **QC Bottleneck (Cycle Time 2.5 วินาที/ชิ้น):** ตรวจไม่ทันความเร็วของเครื่องปั๊มรีดความเร็วสูง ส่งผลให้ตรวจได้เพียงแบบสุ่ม (Sample Inspection 5-10%)
3. **Cost of Poor Quality (COPQ):** เสียค่าปรับเคลมสินค้าและค่าส่งของด่วนทางเครื่องบินเฉลี่ย 450,000 บาท/เดือน

---

## 2. Value Proposition & Economic Feasibility

ระบบ **3Eyes Anomaly Detection** นำเทคโนโลยี Computer Vision ติดตั้ง 3 มุมมอง (`Top`, `Side_1`, `Side_2`) ควบคู่กับ Deep Learning Unsupervised Anomaly Detection:

- **100% In-line Inspection:** ตรวจน็อตทุกตัวบนสายพานด้วยความเร็ว **120 ms/ชิ้น** (เร็วกว่าเดิม 20 เท่า)
- **Eliminate Blind Spots:** กล้อง 3 มุมมองครอบคลุมทั้งหัวน็อต เกลียว 360 องศา และปลายเกลียว
- **Direct Payback:** จุดคุ้มทุน (Payback Period) ภายใน **4.2 เดือน** ประหยัดต้นทุนเคลมกว่า **5.1 ล้านบาทต่อปี**

---

## 3. System Architecture & Data Flow

```
   [ Fastener Conveyor Line ] 
               │
      (High-speed Trigger)
               ▼
   [ 3-Camera Vision Rig ] ───► Top (Head Crack)
               │               Side_1 (Thread Damage)
               │               Side_2 (Burr & Pitch)
               ▼
   [ Edge GPU Inference ] ───► Anomaly Scoring (0.00 - 1.00)
               │
               ├─── Pass (Score < 0.75) ───► Conveyor to Packing
               └─── Reject (Score >= 0.75) ──► Pneumatic Blow-off
               │
               ▼
   [ Data Warehouse / Log ] ───► raw_inspection_log.csv
               ▼
   [ Storytelling BI Dashboard ] ─► Executive KPI, Anomaly Diagnosis, Action Alerts
```

---

## 4. Repository Structure

```
Datawarehouse-miniproject/
├── README.md                               # Project documentation & business narrative
├── data/
│   └── raw_inspection_log.csv              # Production mock inspection dataset (15 records)
├── docs/
│   ├── data_dictionary.md                  # Detailed data schema and metric formulas
│   └── business_intelligence_strategy.md   # Executive strategy & Codebase analysis report
└── dashboard/
    ├── index.html                          # Interactive Web Dashboard (Storytelling UI with Chart.js)
    └── storytelling_dashboard_spec.md      # BI layout, visuals, color tokens, and narrative acts
```

---

## 5. Live Interactive Dashboard (วิธีเปิดใช้งาน)

ระบบมี Interactive Storytelling Dashboard พัฒนาด้วย HTML5, CSS3 (Vanilla Dark Mode), และ Chart.js:
- **เปิดไฟล์โดยตรง:** ดับเบิลคลิกเปิดไฟล์ [`dashboard/index.html`](dashboard/index.html) บนเว็บเบราว์เซอร์ (Chrome / Edge / Firefox)
- **ฟีเจอร์เด่น:**
  - ตัวเลข KPI รวมแบบสด (1.24M ชิ้น, 28 PPM, คืนทุน 4.2 เดือน)
  - กราฟเปรียบเทียบการตรวจจับ 3 มุมกล้อง (Top vs Side vs Tip)
  - Heatmap ความหนาแน่นของตำหนิแยกตามกะการทำงาน (Morning/Afternoon/Night Shift)
  - กราฟเส้นแจกแจง Anomaly Score และเส้นตัดแบ่งความผิดปกติ (Operating Threshold 0.75)
  - กราฟแท่งก่อน-หลัง (Before vs After) และเส้นทางคืนทุน (Payback Curve)
  - ตารางประวัติการตรวจสอบย้อนกลับ (Traceability Log) อ้างอิงมาตรฐาน IATF 16949


1. **Executive Hook:** รวมตัวเลข Core Business KPIs (Total Inspected, PPM Defect Slip Rate, Cost Saved)
2. **Anomaly Diagnosis:** ชาร์ตแจกแจงมุมมองกล้อง (`Top`, `Side_1`, `Side_2`) เทียบประเภทตำหนิ และ Heatmap แยกตามกะการทำงาน (Shift)
3. **Business Impact:** เปรียบเทียบผลลัพธ์ Before vs After (QC Time ลดลง 95%, เคลมลดลง 96%) พร้อมกราฟคืนทุน
4. **Actionable Insights:** การแจ้งเตือนบำรุงรักษาเชิงรุก (Predictive Die Replacement) และการปรับแต่งค่า Threshold

---

## 6. Team Members & Roles

| No. | Student ID | Full Name (ชื่อ-นามสกุล) | Project Role | Responsibilities (หน้าที่รับผิดชอบ) |
| :-: | :---: | :--- | :--- | :--- |
| 1 | 67160366 | นายรณชัย ขาวสะอาด | **Leader** | บริหารจัดการภาพรวมโครงการ, ควบคุมกำหนดการ (Timeline), วิเคราะห์ Business Strategy และความคุ้มค่าทางเศรษฐศาสตร์ (ROI/Payback) |
| 2 | 67160333 | นายดุลยุตม์ เลื่องสุนทร | **Tech Leader** | วางสถาปัตยกรรมระบบทางเทคนิค, ควบคุมการพัฒนาโมเดล AI Anomaly Detection, ออกแบบ Pipeline และสถาปัตยกรรมข้อมูล |
| 3 | 67160157 | นายสุริเยนทร์ กงทิพย์ | **Tech** | พัฒนาและเชื่อมต่อระบบการประมวลผลข้อมูล (Data Processing), จัดทำ Data Warehouse Schema, พัฒนาแดชบอร์ด BI และ Interactive Visualizations |
| 4 | 67160218 | นายธนโชติ โสอุบล | **QA Leader** | วางแผนและควบคุมกระบวนการทดสอบคุณภาพระบบ (Quality Assurance), ตรวจสอบความถูกต้องของโมเดลและการแจ้งเตือน (False Alarm / Escape Rate) |
| 5 | 67160167 | นายชนาวิทย์ ที่ตั้ง | **QA** | ทดสอบ Data Integrity, ตรวจสอบความสอดคล้องของชุดข้อมูล Log, ประเมินความถูกต้องของการจำแนกประเภทตำหนิ (Defect Classification) |
| 6 | 67160384 | นายอรรถพล แก้วสม | **QA** | ดำเนินการทดสอบระบบแบบ End-to-End, ทดสอบ Edge Cases ของโมเดลในสภาพแวดล้อมจำลอง และจัดทำรายงานผลการทดสอบ (Test Report) |
| 7 | 67160370 | นายวิชิต สุวัฒน์ธนากร | **Support Leader** | ดูแลและประสานงานฝ่ายสนับสนุน, จัดทำเอกสารคู่มือทางเทคนิค (Technical Documentation), บริหารจัดการสภาพแวดล้อมและ Resource โครงการ |
| 8 | 67160232 | นายมงคล ผสมทรัพย์ | **Support** | จัดเตรียมข้อมูลสนับสนุน, ออกแบบสื่อนำเสนอ (Presentation Deck), จัดทำเอกสาร Storytelling และ Data Dictionary |
| 9 | 67160335 | นายเทวนาถ สระทองแดง | **Support** | บันทึกรายงานการประชุม, สนับสนุนการเตรียมตัวนำเสนอ, จัดการ Version Control และประสานงานส่งมอบผลงานโครงงาน |

---

## 7. Quick Start & Exploration

1. ดูชุดข้อมูลตัวอย่างที่: [`data/raw_inspection_log.csv`](data/raw_inspection_log.csv)
2. ศึกษา Data Schema อย่างละเอียดที่: [`docs/data_dictionary.md`](docs/data_dictionary.md)
3. ศึกษารายละเอียดการออกแบบ Dashboard และคู่มือสีที่: [`dashboard/storytelling_dashboard_spec.md`](dashboard/storytelling_dashboard_spec.md)
