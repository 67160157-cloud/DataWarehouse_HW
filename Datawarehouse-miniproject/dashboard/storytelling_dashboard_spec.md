# Storytelling Dashboard Design Specification: 3Eyes Industrial QC

**Concept:** "Zero-Defect Journey: จากต้นทุนหลุดรอดสู่ความแม่นยำระดับสายการผลิตยานยนต์"  
**Audience:** Executive Committee, Plant Manager, Quality Assurance Director  
**Theme & Styling:** Industrial High-Tech Dark / Midnight Slate Theme (พื้นหลัง Slate-900 `#0F172A`, การ์ด Slate-800 `#1E293B`) ให้ความรู้สึกพรีเมียม สบายตาในการมอนิเตอร์บน Control Room จอใหญ่

---

## Story Architecture (The 4 Narrative Acts)

```
[ Act 1: Executive Hook ]  ---> ภาพรวมความสำเร็จทางการเงินและประสิทธิภาพ QC
         │
[ Act 2: Anomaly Diagnosis ] -> เจาะลึกมุมกล้องและสาเหตุรากเหง้า (Root-Cause)
         │
[ Act 3: Business Impact ]  ---> พิสูจน์ผลลัพธ์ Before vs After และ ROI
         │
[ Act 4: Actionable Insights ]-> คำสั่งการปรับปรุงแบบเจาะจงทันทีในระดับปฏิบัติการ
```

---

## Layout Breakdown

### Act 1: Executive Hook (Top Banner & Hero Cards)
*เป้าหมาย: ดึงดูดสายตาผู้บริหารภายใน 5 วินาทีแรก ด้วยตัวเลข KPI ทางธุรกิจที่เห็นผลลัพธ์ชัดเจน*

| Visual Element | Chart Type | Indicator / Field | Color Palette | Rationale & Narrative |
| :--- | :--- | :--- | :--- | :--- |
| **Total Inspected** | KPI Big Number Card with sparkline | `COUNT(Lot_ID)` | Electric Blue (`#38BDF8`) | บอกปริมาณงาน Throughput ทั้งหมด แสดงความเสถียรของไลน์ |
| **Defect Slip Rate** | KPI Badge Card | Escaped PPM (PPM = Parts Per Million) | Emerald Green (`#10B981`) | ลดลงจาก 1,450 PPM เหลือ 28 PPM (เป้าหมายยานยนต์ < 50 PPM) |
| **Inspection Throughput** | Speedometer / Radial Gauge | `Inspection_Time_ms` (~120ms vs 2,500ms target) | Amber / Cyan | แสดงความเร็วกว่าคนตรวจ 20 เท่า ไม่เกิด Bottleneck |
| **Est. Net Cost Saved** | Value Card with Monthly Trend | `SUM(Claim Reduction - Opex)` | Bright Mint (`#34D399`) | ตอกย้ำมูลค่าเงินคืนทุนที่โรงงานประหยัดได้จริง (บาท/เดือน) |

---

### Act 2: Anomaly Diagnosis (Drill-Down Matrix)
*เป้าหมาย: ตอบคำถามว่า "ของเสียเกิดที่ไหน เกิดด้วยมุมกล้องใด และเกิดช่วงเวลาใดมากที่สุด"*

1. **Camera Angle Detection Power (Radar Chart / Stacked Horizontal Bar):**
   - **X-axis/Metric:** Defect Detection Count & Anomaly Score Density
   - **Dimension:** `Camera_Angle` (`Top`, `Side_1`, `Side_2`)
   - **Color:** 
     - `Top`: Cyan (`#06B6D4`) -> เชี่ยวชาญ `Head Crack` (94% detection)
     - `Side_1`: Purple (`#8B5CF6`) -> เชี่ยวชาญ `Thread Damage` (98% detection)
     - `Side_2`: Orange (`#F59E0B`) -> เชี่ยวชาญ `Burr` บริเวณปลายเกลียวและใต้คอแหวน
   - **Business Rationale:** พิสูจน์ว่าทำไมต้องใช้ "3 มุมมอง" หากใช้เพียงกล้อง Top view เดียว จะพลาด Thread Damage ถึง 68%!

2. **Defect Distribution by Shift & Line (Heatmap Matrix):**
   - **Rows:** `Production_Line_ID` (Line A, B, C)
   - **Columns:** `Shift` (Morning, Afternoon, Night)
   - **Value:** Defect Count & Override Rate
   - **Color Scale:** Light Gray (0 defects) ถึง Crimson Red (`#EF4444`) (High defect density)
   - **Business Insight:** พบว่า Line-B ใน Night Shift มี Defect สูงผิดปกติ บ่งชี้ปัญหาเรื่อง Tool wear หรืออุณหภูมิของแม่พิมพ์รีดเกลียว

3. **Anomaly Score Distribution (Histogram / Ridge Plot):**
   - **X-axis:** `Anomaly_Score` (0.0 to 1.0)
   - **Threshold Cutoff Line:** Vertical Dashed Line at `0.75` (Yellow warning at `0.70`, Red cutoff at `0.75`)
   - **Purpose:** ชี้ให้เห็นการแยกตัว (Separability) ที่ชัดเจนระหว่าง Normal bolts (<0.2) กับ Defective bolts (>0.75)

---

### Act 3: Business Impact (Before vs After Evidence)
*เป้าหมาย: เปรียบเทียบความคุ้มค่าด้านต้นทุนและเวลา เพื่อความมั่นใจในการขยายสเกลการลงทุน (Rollout)*

1. **Before vs After Dual Bar Chart (Side-by-Side Comparison):**
   - **Metric 1: QC Cycle Time per Unit:** 2,500 ms (คนส่องแว่นขยาย) $\rightarrow$ **120 ms (ระบบ 3Eyes)** *(ลดลง 95.2%)*
   - **Metric 2: Defect Escape Rate:** 1.5% $\rightarrow$ **0.03%** *(ลดลง 98%)*
   - **Metric 3: Customer Claim Cost (THB/Month):** 450,000 THB $\rightarrow$ **18,000 THB** *(ลดลง 96%)*
   - **Color:** Muted Slate (`#64748B`) สำหรับ Before vs Vivid Emerald (`#10B981`) สำหรับ After

2. **Cumulative Net Savings & Payback Curve (Waterfall Chart / Cumulative Line):**
   - แสดงจุดคุ้มทุน (Breakeven Point) ที่ 4.2 เดือน หลังจากหักลบค่า Hardware (กล้อง 3 ตัว + Edge GPU Box)

---

### Act 4: Actionable Insights & Prescriptive Decisions
*เป้าหมาย: เปลี่ยนข้อมูลให้เป็น Action เชิงรุกสำหรับหัวหน้าแผนกและวิศวกรบำรุงรักษา*

- **Card 1: Machine Maintenance Alert:**
  - *Finding:* สายการผลิต `LINE-B` มีสัดส่วน `Thread Damage` สูงขึ้น 24% ใน 48 ชม. ที่ผ่านมา
  - *Action:* แจ้งเตือนฝ่ายบำรุงรักษาเปลี่ยนชุดลูกรีดเกลียว (Threading Die) ภายในกะถัดไป
- **Card 2: Threshold Optimization Recommendation:**
  - *Finding:* ชิ้นส่วนประเภท `Head Crack` มี Anomaly Score เฉลี่ย 0.68 - 0.72 ซึ่งถูก Inspector Override เป็น Pass บ่อยครั้ง
  - *Action:* ปรับ Calibration ลด False Rejection โดยปรับ Sensitivity เกณฑ์มุม `Top` ให้มีความแม่นยำจำเพาะยิ่งขึ้น
- **Card 3: Supplier Raw Material Variance:**
  - *Finding:* ล็อต `LOT-20261005-02` มีอัตราเศษเสี้ยน (`Burr`) สูง
  - *Action:* ส่งเคลมและประสานงานกับซัพพลายเออร์ตัดลวดเหล็ก (Wire Rod) เพื่อตรวจสอบความคมของใบมีดตัด
