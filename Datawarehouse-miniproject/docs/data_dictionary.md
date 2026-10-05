# Data Dictionary & Schema Specification (DW Mini-Project)

**ระบบ:** 3Eyes Anomaly Detection - Industrial Fastener Quality Control Analytics  
**วิชา:** Business Idea Creation (Data Warehouse & BI Mini-Project)  
**การเชื่อมโยงกับโค้ดจริง:** ต่อยอดจาก Data Model ใน `3eyes-anomaly-detection-master` (`src/threeeyes/`, `data/manifest.csv`) เพื่อนำผลการตรวจระดับภาพและน็อตเข้าสู่ Data Warehouse สำหรับจัดทำ Storytelling Dashboard

---

## 1. ผังโครงสร้างตารางข้อมูล (Data Schema: `fact_fastener_inspection`)

| Column Name | Data Type | Constraint / Domain | คำอธิบายฟิลด์ | Business Usage & Metric Link |
| :--- | :--- | :--- | :--- | :--- |
| `Timestamp` | TIMESTAMP (ISO 8601) | NOT NULL | วัน-เวลาที่กล้องบันทึกภาพและประมวลผล | แนวโน้มรายชั่วโมง/กะ/วัน |
| `Bolt_ID` | VARCHAR(30) | NOT NULL | รหัสประจำตัวชิ้นงานน็อต (Part-level ID) | **Traceability** ตามข้อกำหนด IATF 16949 |
| `Production_Line_ID` | VARCHAR(10) | LINE-A, LINE-B, LINE-C | รหัสสายการผลิตหรือเครื่องจักรขึ้นรูป/รีดเกลียว | วิเคราะห์ปัญหาจำเพาะเครื่องจักร (Machine Wear) |
| `Shift` | VARCHAR(15) | Morning, Afternoon, Night | กะการทำงานของพนักงานหน้างาน | วิเคราะห์ความล้าของพนักงาน (Inspector Fatigue) |
| `Camera_View_Type` | VARCHAR(15) | Top_Head, Side_Radial, Bottom_Tip | มุมกล้องตรวจจับ (หัวน็อต / ข้างหมุนรอบ / ปลายเกลียว) | วิเคราะห์จุดบอดและประสิทธิภาพจำแนกแต่ละมุม |
| `Angle_Index` | INTEGER | 0 ถึง 7 (8 มุมมาตรฐานตาม Protocol) | ลำดับมุมถ่าย (0–5: ด้านข้าง, 6: ปลาย/ก้น, 7: หัว) | อ้างอิงตรงกับชุดมุมใน `docs/01_DATASET.md` |
| `Defect_Type` | VARCHAR(25) | Normal, Thread Damage, Head Crack, Burr, etc. | ประเภทตำหนิที่ตรวจพบ (อิง 8 ชนิดใน `docs/00_BRIEF.md`) | Pareto Analysis & Root Cause Analysis |
| `Anomaly_Score` | FLOAT (3 Decimals) | 0.000 ถึง 1.000 | คะแนนความผิดปกติจาก PatchCore / Feature Scorer | ปรับแต่ง Sensitivity & Operating Threshold |
| `Anomaly_Threshold` | FLOAT (3 Decimals) | ค่ามาตรฐาน 0.750 | ค่าเกณฑ์ตัดสิน Pass/Reject ที่ระบบตั้งไว้ | อ้างอิงจุดตัดการทำงาน (Operating Point) |
| `Inspection_Result` | VARCHAR(10) | Pass, Reject | ผลการตัดสินขั้นแรกโดยระบบ AI | คำนวณ Scrap Rate, First Pass Yield (FPY) |
| `Inspector_Override` | VARCHAR(5) | Yes, No | พนักงานหน้างานกลับคำตัดสิน (Human Intervention) | วัด False Alarm Rate / ความมั่นใจของผู้ใช้งาน |
| `Override_Reason` | VARCHAR(100) | NULL หรือข้อความเหตุผล | เหตุผลที่พนักงานปรับผล (เช่น แสงสะท้อน, ฝุ่นเกาะ) | ปรับปรุงโมเดลและฟิลเตอร์แสงสะท้อนรอบถัดไป |
| `Inference_Time_ms` | INTEGER | Milliseconds (> 0) | เวลาประมวลผลต่อภาพ (ms) | ตรวจสอบ Throughput ไม่ให้เป็น Bottleneck |
| `Part_Cost_THB` | DECIMAL(8,2) | 4.50 บาท/ชิ้น | ต้นทุนการผลิตน็อตต่อชิ้น | ประเมินต้นทุนรวมของล็อต |
| `Scrap_Cost_THB` | DECIMAL(8,2) | 0.00 หรือ 4.50 บาท | มูลค่าสูญเสียเมื่อชิ้นงานถูก Reject | คำนวณ Cost of Poor Quality (COPQ) |

---

## 2. สูตรและตรรกะการคำนวณตัวชี้วัด (KPI Formulas)

1. **Defect Escape Rate (PPM) - อัตราของเสียหลุดรอด:**
   $$\text{Slip Rate (PPM)} = \left(\frac{\text{Defective Parts Escaped to Customer}}{\text{Total Shipped Units}}\right) \times 1,000,000$$
   *เป้าหมาย: ลดจาก 1,500 PPM เหลือ < 30 PPM*

2. **False Rejection Rate (FRR) / False Alarm Rate:**
   $$\text{FRR} = \left(\frac{\text{Number of Overrides (Reject } \rightarrow \text{ Pass)}}{\text{Total Rejects by Model}}\right) \times 100\%$$

3. **Operating Cycle Time Speedup:**
   $$\text{Speedup Factor} = \frac{\text{Manual Cycle Time (2,500 ms)}}{\text{3Eyes Vision Time (120 ms)}} \approx 20.8\times$$

4. **Monthly Cost Savings (THB):**
   $$\text{Net Monthly Savings} = \Delta\text{Claim Cost} + \Delta\text{QC Labor Cost} - \text{System Maintenance (Opex)}$$
