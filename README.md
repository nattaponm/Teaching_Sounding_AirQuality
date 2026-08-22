# Teaching Sounding for Air Quality

ชุดการเรียนการสอนการใช้ข้อมูล **Radiosonde Sounding** เพื่อวิเคราะห์โครงสร้างบรรยากาศ เสถียรภาพบรรยากาศ และตัวแปรอุตุนิยมวิทยาที่เกี่ยวข้องกับคุณภาพอากาศ โดยใช้ Python บน Google Colab

Repository นี้ออกแบบสำหรับนิสิตระดับปริญญาตรีชั้นปีที่ 3–4 ในสาขาสิ่งแวดล้อม ภูมิศาสตร์ ภูมิสารสนเทศ วิทยาศาสตร์บรรยากาศ หรือสาขาที่เกี่ยวข้อง และสามารถใช้เป็นพื้นฐานสำหรับการเรียนระดับบัณฑิตศึกษาได้

---

## วัตถุประสงค์ของชุดการสอน

ชุด Notebook นี้มีเป้าหมายให้นิสิตสามารถ

1. เข้าใจโครงสร้างและตัวแปรของข้อมูล radiosonde sounding
2. ดาวน์โหลดและจัดเตรียมข้อมูลสำหรับการวิเคราะห์อย่างเป็นระบบ
3. ตรวจสอบคุณภาพข้อมูล (Quality Control: QC)
4. วิเคราะห์ vertical atmospheric profile
5. สร้างและอ่านแผนภาพ Skew-T Log-P
6. คำนวณตัวแปรอุณหพลศาสตร์ของบรรยากาศ
7. วิเคราะห์ atmospheric stability และ temperature inversion
8. วิเคราะห์ลมชั้นล่างและการระบายตัวของบรรยากาศ
9. ประมาณ morning mixing-height diagnostic ด้วย Bulk Richardson Number
10. คำนวณ ventilation coefficient
11. วิเคราะห์ความแปรผันตามเวลาและเปรียบเทียบ March–April 2024
12. ใช้ descriptive statistics และ inferential statistics อย่างเหมาะสม
13. เข้าใจข้อจำกัดของ statistical significance, correlation และ temporal autocorrelation
14. เตรียมพื้นฐานสำหรับนำข้อมูล PM2.5 มาวิเคราะห์ร่วมกับ sounding ในขั้นต่อไป

แนวคิดของชุดการสอนคือ

```text
Data
  ↓
Quality Control
  ↓
Vertical Atmospheric Structure
  ↓
Thermodynamics
  ↓
Stability / Inversion / Mixing
  ↓
Temporal Statistics
  ↓
Air-Quality Interpretation
```

---

# ข้อมูลที่ใช้ในการเรียน

## สถานีตรวจอากาศชั้นบน

- **Station:** Chiang Mai
- **WMO Station ID:** 48327
- **ช่วงข้อมูล:** 1 March – 30 April 2024
- **เวลาที่ใช้:** 00 UTC
- **เวลาไทย:** 07:00 ICT
- **จำนวน sounding:** 61 soundings
- March 2024 = 31 soundings
- April 2024 = 30 soundings
- Data availability ของ 00 UTC ในช่วงที่ศึกษา = 100%

ข้อมูลต้นทางมาจาก University of Wyoming Atmospheric Science Radiosonde Archive และถูกจัดเตรียมเป็น teaching dataset เพื่อให้นิสิตทุกคนใช้ข้อมูลชุดเดียวกัน

---

## หมายเหตุสำคัญเกี่ยวกับเวลา

ประเทศไทยใช้เวลา

\[
ICT = UTC + 7
\]

ดังนั้น

\[
00\ UTC = 07{:}00\ ICT
\]

ข้อมูลใน repository นี้จึงสะท้อน **โครงสร้างบรรยากาศช่วงเช้า (morning atmospheric environment)**

การคำนวณ CAPE, CIN, inversion, mixing height หรือ atmospheric stability จากข้อมูลชุดนี้จึงไม่ควรถูกตีความว่าเป็นค่าตัวแทนสูงสุดของทั้งวัน

ตัวอย่างเช่น ค่าที่คำนวณจาก Bulk Richardson Number ควรเรียกว่า

> **morning mixing-height diagnostic**

ไม่ควรเรียกว่า daily maximum PBL height โดยอัตโนมัติ

---

# โครงสร้างข้อมูล Sounding

ข้อมูล sounding เป็นข้อมูลแนวดิ่งของบรรยากาศ

หนึ่ง sounding ประกอบด้วยหลายระดับความสูง ดังนั้น

```text
1 sounding ≠ 1 row
```

แต่เป็น

```text
1 sounding = many vertical atmospheric levels
```

ตัวแปรหลักใน teaching dataset ได้แก่

| Variable | ความหมาย | หน่วย |
|---|---|---|
| `pressure_hPa` | Atmospheric pressure | hPa |
| `height_m` | Geopotential height | m |
| `temperature_C` | Air temperature | °C |
| `dewpoint_C` | Dew-point temperature | °C |
| `relative_humidity_pct` | Relative humidity | % |
| `mixing_ratio_gkg` | Water-vapor mixing ratio | g kg⁻¹ |
| `wind_direction_deg` | Wind direction | degree |
| `wind_speed_ms` | Wind speed | m s⁻¹ |
| `theta_K` | Potential temperature | K |
| `theta_e_K` | Equivalent potential temperature | K |
| `theta_v_K` | Virtual potential temperature | K |

---

# ไฟล์ข้อมูลใน Repository

ไฟล์ข้อมูลหลักประกอบด้วย

### `chiangmai_48327_sounding_20240301_20240430.csv.gz`

Combined radiosonde dataset สำหรับช่วง March–April 2024

ไฟล์ถูกบีบอัดด้วย gzip และสามารถอ่านได้โดยตรงด้วย

```python
pd.read_csv(
    "filename.csv.gz",
    compression="gzip"
)
```

### `launch_manifest_48327_20240301_20240430.csv`

บันทึกสถานะการดาวน์โหลด sounding ในแต่ละวันและแต่ละเวลา

ใช้สำหรับตรวจสอบ

- วันที่ร้องขอข้อมูล
- UTC hour
- download status
- data availability
- จำนวน vertical levels
- structural QC เบื้องต้น

### `station_metadata_48327.csv`

Metadata ของสถานีและชุดข้อมูล

### `download_report_48327_20240301_20240430.txt`

รายงานสรุปผลการดาวน์โหลดข้อมูล

### `README_DATASET.md`

คำอธิบายเฉพาะของ teaching dataset

---

# ลำดับการใช้ Notebook

แนะนำให้รัน Notebook ตามลำดับต่อไปนี้

---

## Notebook 00 — Data Preparation from University of Wyoming

### `00_download_ChiangMai_48327_Sounding_MarApr2024.ipynb`

**สำหรับผู้สอน / ผู้ที่ต้องการเรียนรู้ขั้นตอนการสร้าง dataset**

Notebook นี้ใช้สำหรับ

- ติดต่อ University of Wyoming radiosonde archive
- ดาวน์โหลด sounding
- ตรวจสอบ 00, 06 และ 12 UTC
- แยก raw data และ processed data
- ตรวจสอบ download availability
- สร้าง manifest
- รวม sounding เป็น combined dataset
- สร้าง metadata
- สร้าง ZIP archive
- เตรียมไฟล์สำหรับ GitHub

สำหรับการเรียนในชั้นเรียนทั่วไป นิสิตไม่จำเป็นต้องรัน Notebook 00 เพราะ teaching dataset ได้ถูกเตรียมไว้ใน repository แล้ว

---

## Notebook 01 — Download Teaching Dataset from GitHub

### `01_get_ChiangMai_sounding_from_GitHub.ipynb`

**Notebook เริ่มต้นสำหรับนิสิต**

ทำหน้าที่

- mount Google Drive
- ดาวน์โหลด dataset จาก GitHub
- ตรวจ metadata
- ตรวจ data availability
- เลือกเฉพาะ 00 UTC
- ตรวจว่าได้ 61 soundings
- ตรวจ March = 31 วัน
- ตรวจ April = 30 วัน
- ทำ structural QC เบื้องต้น
- สร้าง analysis-ready teaching dataset

ไฟล์ที่สร้าง:

```text
/content/drive/MyDrive/Teaching_Sounding_AirQuality/
01_data/chiangmai_48327_00UTC_20240301_20240430.csv.gz
```

Notebook 02–05 จะใช้ไฟล์นี้เป็น input หลัก

---

## Notebook 02 — Understanding Sounding Data and Quality Control

### `02_understanding_sounding_data_and_QC.ipynb`

หัวข้อสำคัญ

- โครงสร้าง radiosonde sounding
- Pressure และ geopotential height
- Temperature และ dew point
- Relative humidity
- Mixing ratio
- Wind speed และ wind direction
- Potential temperature
- Missing data
- Structural quality control
- Vertical atmospheric profiles
- Lower-tropospheric structure
- การเปรียบเทียบ sounding หลายวัน

แนวคิดสำคัญ:

> ก่อนคำนวณดัชนีทางอุตุนิยมวิทยา ผู้วิเคราะห์ควรเข้าใจข้อมูลจริงและตรวจ QC ก่อนเสมอ

---

## Notebook 03 — Skew-T and Atmospheric Thermodynamics

### `03_SkewT_and_atmospheric_thermodynamics.ipynb`

ใช้ MetPy เพื่อศึกษา

- Skew-T Log-P
- environmental temperature
- dew-point profile
- dry adiabats
- moist adiabats
- mixing-ratio lines
- parcel profile
- Lifted Condensation Level (LCL)
- Level of Free Convection (LFC)
- Equilibrium Level (EL)
- Surface-Based CAPE / CIN
- Mixed-Layer CAPE / CIN
- Most-Unstable CAPE / CIN
- Precipitable Water (PWAT)
- Wind barbs

Notebook ยังสร้าง daily thermodynamic diagnostics สำหรับ sounding ทั้ง 61 วัน

ตัวอย่าง output:

```text
03_daily_thermodynamic_diagnostics_48327_00UTC_MarApr2024.csv
```

---

## Notebook 04 — Stability, Inversion and Air-Pollution Meteorology

### `04_stability_inversion_and_air_pollution.ipynb`

Notebook นี้เป็นส่วนสำคัญในการเชื่อม atmospheric sounding กับ air-quality meteorology

หัวข้อประกอบด้วย

- Mean Sea Level (MSL) และ Above Ground Level (AGL)
- Environmental lapse rate
- Potential-temperature gradient
- Static stability
- Temperature inversion
- Inversion base
- Inversion top
- Inversion depth
- Temperature increase across inversion
- Inversion strength
- Brunt–Väisälä frequency
- Wind components \(u,v\)
- Mean wind 0–500 m
- Mean wind 0–1 km
- Bulk Richardson Number
- Morning mixing-height diagnostic
- Ventilation coefficient

### Temperature inversion

Temperature inversion คือชั้นที่

\[
\frac{dT}{dz}>0
\]

กล่าวคืออุณหภูมิเพิ่มขึ้นเมื่อสูงขึ้น

ใน Notebook ใช้ teaching thresholds เพื่อหลีกเลี่ยงการตรวจ noise เป็น inversion:

```text
minimum inversion depth = 100 m
minimum ΔT = 0.5 °C
```

ค่าดังกล่าวเป็นตัวเลือกเชิงวิธีการสำหรับการเรียน ไม่ใช่มาตรฐานสากลตายตัว

### Bulk Richardson Number

แนวคิดโดยทั่วไป

\[
Ri_b =
\frac{
(g/\theta_{v0})
[\theta_v(z)-\theta_{v0}]z
}{
[u(z)-u_0]^2+[v(z)-v_0]^2
}
\]

ใช้ threshold

\[
Ri_b \approx 0.25
\]

สำหรับ teaching diagnostic ของ mixing height

### Ventilation coefficient

\[
VC = MH \times \overline{U}
\]

โดย

- \(MH\) = mixing-height diagnostic
- \(\overline{U}\) = mean wind ภายใน mixing layer

Notebook สร้าง output:

```text
04_daily_airpollution_meteorology_48327_00UTC_MarApr2024.csv
```

---

## Notebook 05 — Temporal and Statistical Analysis

### `05_temporal_statistics_MarApr2024.ipynb`

รวมผลจาก Notebook 03 และ 04 เป็น daily analysis table จำนวน 61 วัน

หัวข้อประกอบด้วย

### Descriptive statistics

- mean
- standard deviation
- median
- IQR
- P10
- P25
- P50
- P75
- P90
- skewness

### Temporal analysis

- daily time series
- 3-day rolling mean
- 7-day rolling mean

### March vs April

- boxplot
- empirical cumulative distribution function (ECDF)
- descriptive comparison

### Statistical tests

- Welch's t-test
- Mann–Whitney U test
- effect size
- multiple-testing concept
- Benjamini–Hochberg False Discovery Rate

### Correlation

- Spearman rank correlation
- correlation matrix
- pairwise sample size

### Temporal dependence

- lag-1 autocorrelation
- simple autocorrelation function (ACF)
- approximate effective sample size

Notebook สร้าง merged daily dataset:

```text
05_merged_daily_sounding_diagnostics_48327_MarApr2024.csv
```

ซึ่งสามารถนำไปใช้ต่อกับ PM2.5 ได้

---

# Workflow สำหรับนิสิต

สำหรับการเรียนปกติ ให้เริ่มที่ Notebook 01

```text
01_get_ChiangMai_sounding_from_GitHub.ipynb
                    ↓
02_understanding_sounding_data_and_QC.ipynb
                    ↓
03_SkewT_and_atmospheric_thermodynamics.ipynb
                    ↓
04_stability_inversion_and_air_pollution.ipynb
                    ↓
05_temporal_statistics_MarApr2024.ipynb
```

Notebook แต่ละบทจะบันทึก output ลง Google Drive เพื่อให้ Notebook ถัดไปอ่านได้โดยตรง

---

# Google Drive Folder Structure

หลังจากรัน Notebook 01 โครงสร้างหลักจะเป็น

```text
MyDrive/
└── Teaching_Sounding_AirQuality/
    │
    ├── 00_source_github/
    │
    ├── 01_data/
    │   └── chiangmai_48327_00UTC_20240301_20240430.csv.gz
    │
    ├── 02_metadata/
    │
    ├── 03_output/
    │   ├── 03_daily_thermodynamic_diagnostics_...
    │   ├── 04_daily_airpollution_meteorology_...
    │   └── 05_merged_daily_sounding_diagnostics_...
    │
    └── 04_figures/
```

---

# Software and Python Libraries

Notebook ถูกออกแบบสำหรับ **Google Colab**

Python libraries หลักที่ใช้ ได้แก่

- pandas
- NumPy
- Matplotlib
- SciPy
- MetPy
- requests

นิสิตไม่จำเป็นต้องติดตั้ง Python บนเครื่องส่วนตัว หากใช้ Google Colab

---

# หลักการ Quality Control

ชุดการสอนเน้นแนวคิดว่า

> **QC flag ก่อน แล้วจึงตัดสินใจว่าจะแก้หรือลบข้อมูลหรือไม่**

ตัวอย่างการตรวจสอบ:

- missing values
- duplicated pressure levels
- pressure ordering
- height ordering
- \(T_d \le T\)
- RH range
- vertical profile completeness
- derived-variable missing values

ไม่ควรใช้วิธี `dropna()` กับทุกตัวแปรทั้ง sounding โดยไม่มีเหตุผล เพราะอาจทำให้สูญเสียข้อมูลที่ยังใช้ประโยชน์ได้

---

# Scientific Interpretation

## Atmospheric stability

Potential temperature เป็นตัวแปรสำคัญสำหรับการวิเคราะห์ stability

โดยทั่วไป

\[
\frac{d\theta}{dz}>0
\]

สัมพันธ์กับ stable stratification

---

## Inversion และ PM2.5

การพบ temperature inversion ไม่ได้หมายความโดยอัตโนมัติว่า PM2.5 ต้องสูง

ความเข้มข้นของ PM2.5 ยังขึ้นกับ

- emission sources
- biomass burning
- regional transport
- wind
- atmospheric chemistry
- relative humidity
- precipitation
- deposition
- boundary-layer evolution

ดังนั้น sounding ใช้บอก

> **meteorological conditions favorable or unfavorable for dispersion**

ไม่ใช่ใช้แทน pollutant observations

---

# Statistical Interpretation

## Statistical significance ไม่เท่ากับ Physical significance

ถ้า

\[
p<0.05
\]

ไม่ได้หมายความว่า

- effect มีขนาดใหญ่
- มีความสำคัญทางกายภาพเสมอ
- เกิด causal relationship
- \(H_0\) มีโอกาสเป็นจริง 5%

ควรพิจารณาร่วมกับ

- descriptive statistics
- distribution
- effect size
- atmospheric physics

---

## Correlation ไม่เท่ากับ Causation

\[
correlation \neq causation
\]

ตัวอย่างเช่น ventilation coefficient มี mixing height อยู่ในสมการ ดังนั้น correlation ระหว่างสองตัวแปรนี้บางส่วนเกิดจาก **mathematical coupling**

---

## Temporal Autocorrelation

ข้อมูลบรรยากาศรายวันอาจมี persistence

\[
X_t \not\perp X_{t-1}
\]

ดังนั้น 61 daily observations ไม่จำเป็นต้องเท่ากับ 61 independent pieces of information

Notebook 05 จึงแนะนำแนวคิด

- lag-1 autocorrelation
- ACF
- effective sample size

เพื่อให้นิสิตเห็นข้อจำกัดของ independent-sample statistical tests

---

# March vs April 2024

การเปรียบเทียบในชุดการสอนนี้ควรเรียกว่า

> **March–April comparison within 2024**

ไม่ควรเรียกว่า climatological seasonal comparison เพราะใช้ข้อมูลเพียงหนึ่งปี

ข้อมูลประกอบด้วย

```text
March 2024 : n = 31
April 2024 : n = 30
Total      : n = 61
```

---

# แนวทางการทำแบบฝึกหัด

แต่ละ Notebook มีส่วน Exercise เพื่อให้นิสิต

- เปลี่ยนวันที่
- เปลี่ยน parameter
- เปรียบเทียบ profiles
- ทดสอบ inversion threshold
- ทดสอบ Bulk Richardson threshold
- เปรียบเทียบ CAPE definitions
- เปรียบเทียบ March–April
- วิเคราะห์ correlation
- วิเคราะห์ temporal autocorrelation
- อธิบายข้อจำกัดของผลลัพธ์

เป้าหมายไม่ใช่เพียงให้ code ทำงาน แต่ให้นิสิตสามารถตอบได้ว่า

> **ทำไมจึงใช้วิธีนี้ และผลลัพธ์มีความหมายทางกายภาพอย่างไร**

---

# Suggested Learning Sequence

### Level 1 — Data

```text
Notebook 01–02
```

เข้าใจข้อมูล, QC และ atmospheric profiles

### Level 2 — Atmospheric Physics

```text
Notebook 03
```

เข้าใจ Skew-T, parcel theory และ thermodynamics

### Level 3 — Air-Pollution Meteorology

```text
Notebook 04
```

เข้าใจ stability, inversion, mixing และ ventilation

### Level 4 — Data Analysis

```text
Notebook 05
```

วิเคราะห์ temporal variability และ statistics

---

# Future Extension: PM2.5 Mini Research Project

ไฟล์

```text
05_merged_daily_sounding_diagnostics_48327_MarApr2024.csv
```

สามารถนำไปรวมกับข้อมูล PM2.5 จาก Air4Thai เพื่อศึกษา

\[
PM_{2.5}
\leftrightarrow
Mixing\ Height
\]

\[
PM_{2.5}
\leftrightarrow
Inversion\ Strength
\]

\[
PM_{2.5}
\leftrightarrow
Ventilation\ Coefficient
\]

\[
PM_{2.5}
\leftrightarrow
PWAT
\]

\[
PM_{2.5}
\leftrightarrow
Low\ Level\ Wind
\]

และสามารถต่อยอดด้วยข้อมูล

- hotspot / biomass burning
- surface meteorology
- precipitation
- ERA5
- back trajectories
- emission information

เพื่อพัฒนาเป็น mini research project ด้าน air-quality meteorology

---

# Reproducibility

Repository นี้ออกแบบให้ทุกคนใช้ teaching dataset เดียวกัน

workflow หลักคือ

```text
GitHub teaching dataset
        ↓
Google Drive
        ↓
Notebook 02–05
        ↓
Derived CSV tables
        ↓
Figures
        ↓
Statistical analysis
```

วิธีนี้ช่วยให้

- ตรวจสอบขั้นตอนย้อนหลังได้
- ผลลัพธ์ทำซ้ำได้
- ลดความแตกต่างจากการดาวน์โหลดข้อมูลคนละช่วงเวลา
- แยก data acquisition ออกจาก scientific analysis

---

# ข้อควรระวัง

1. ข้อมูล sounding เป็น snapshot ของเวลาหนึ่ง ไม่ใช่สภาวะของทั้งวัน
2. 00 UTC ที่เชียงใหม่คือประมาณ 07:00 ICT
3. CAPE เวลาเช้าไม่ใช่ daily maximum CAPE
4. Ri-based mixing height เป็น diagnostic estimate
5. inversion detection ขึ้นกับ algorithm และ thresholds
6. ventilation coefficient ไม่ใช่ตัววัด PM2.5
7. correlation ไม่แสดง causation
8. p-value ไม่แสดงขนาดของผล
9. temporal autocorrelation ควรถูกพิจารณาในการวิเคราะห์ daily data
10. การตีความควรอาศัย atmospheric physics ควบคู่กับ statistics

---

# Data Source

Radiosonde sounding data were obtained from the **University of Wyoming Atmospheric Science Radiosonde Archive** and prepared as a compact teaching dataset for educational analysis.

เมื่อใช้ข้อมูลหรือพัฒนางานวิจัยต่อ ควรระบุ data source และวันที่/ช่วงเวลาที่ใช้ให้ชัดเจน

---

# Repository

**Teaching_Sounding_AirQuality**

https://github.com/nattaponm/Teaching_Sounding_AirQuality

---

# Citation / Acknowledgment

หากนำ Notebook หรือ workflow นี้ไปใช้ในการเรียน การสอน หรือพัฒนางานวิจัยต่อ ควรอ้างถึง repository นี้และแหล่งข้อมูลต้นทางตามความเหมาะสม

ตัวอย่างรูปแบบ acknowledgement:

> Radiosonde data used in this exercise were obtained from the University of Wyoming Atmospheric Science Radiosonde Archive. Data processing and teaching workflows were implemented in Python using open-source scientific libraries.

---

# สำหรับนิสิต

ไม่จำเป็นต้องจำคำสั่ง Python ทุกคำสั่ง

สิ่งสำคัญกว่าคือสามารถอธิบายได้ว่า

1. ตัวแปรแต่ละตัวหมายถึงอะไร
2. ทำไมต้อง QC
3. atmospheric profile บอกอะไร
4. stability และ inversion มีผลต่อ vertical mixing อย่างไร
5. wind มีผลต่อ dispersion อย่างไร
6. สถิติที่ใช้ตอบคำถามอะไร
7. ข้อจำกัดของข้อมูลและวิธีวิเคราะห์คืออะไร

เป้าหมายสุดท้ายคือการเปลี่ยนจาก

```text
"รันโค้ดได้"
```

ไปสู่

```text
"เข้าใจข้อมูล → เข้าใจฟิสิกส์ → วิเคราะห์ได้ → ตีความได้"
```
