# Teaching Sounding for Air Quality
## Chiang Mai, Thailand — March–April 2024

ชุดการเรียนการสอนการใช้ข้อมูล **Radiosonde Sounding** เพื่อวิเคราะห์โครงสร้างบรรยากาศ เสถียรภาพบรรยากาศ การผกผันอุณหภูมิ การผสมในชั้นบรรยากาศ และตัวแปรอุตุนิยมวิทยาที่เกี่ยวข้องกับ **air-quality meteorology** โดยใช้ Python บน **Google Colab**

Repository นี้ออกแบบสำหรับนิสิตระดับปริญญาตรีชั้นปีที่ 3–4 ในสาขา **วิทยาศาสตร์สิ่งแวดล้อม ภูมิศาสตร์ ภูมิสารสนเทศ วิทยาศาสตร์บรรยากาศ และสาขาที่เกี่ยวข้อง** และสามารถใช้เป็นพื้นฐานสำหรับการเรียนระดับบัณฑิตศึกษาได้

> **แนวคิดหลักของชุดการสอน**
>
> `Data → Quality Control → Vertical Structure → Thermodynamics → Stability / Inversion / Mixing → Temporal Statistics → Air-Quality Interpretation`

---

# Start Here

สำหรับนิสิต ให้เริ่มจาก **Notebook 01** แล้วรันตามลำดับจนถึง Notebook 05

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/01_get_ChiangMai_sounding_from_GitHub.ipynb)

**Notebook 00** เป็น Instructor/Data-preparation notebook สำหรับผู้สอนหรือผู้ที่ต้องการเรียนรู้ขั้นตอนการสร้าง teaching dataset จาก University of Wyoming

```text
00  Data acquisition & preparation
             ↓
01  Get teaching dataset from GitHub
             ↓
02  Understand sounding structure & QC
             ↓
03  Skew-T & atmospheric thermodynamics
             ↓
04  Stability, inversion & air-pollution meteorology
             ↓
05  Temporal & statistical analysis
```

---

# Open the Notebooks in Google Colab

| Notebook | File | Main topic | Recommended use | Colab |
|---|---|---|---|---|
| **00** | `00_download_ChiangMai_48327_Sounding_MarApr2024.ipynb` | Data acquisition & preparation | Instructor / optional | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/00_download_ChiangMai_48327_Sounding_MarApr2024.ipynb) |
| **01** | `01_get_ChiangMai_sounding_from_GitHub.ipynb` | Get teaching dataset from GitHub | Start here for students | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/01_get_ChiangMai_sounding_from_GitHub.ipynb) |
| **02** | `02_understanding_sounding_data_and_QC.ipynb` | Understand sounding structure & QC | Core | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/02_understanding_sounding_data_and_QC.ipynb) |
| **03** | `03_SkewT_and_atmospheric_thermodynamics.ipynb` | Skew-T & atmospheric thermodynamics | Core | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/03_SkewT_and_atmospheric_thermodynamics.ipynb) |
| **04** | `04_stability_inversion_and_air_pollution.ipynb` | Stability, inversion & air-pollution meteorology | Core | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/04_stability_inversion_and_air_pollution.ipynb) |
| **05** | `05_temporal_statistics_MarApr2024.ipynb` | Temporal & statistical analysis | Core | [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/05_temporal_statistics_MarApr2024.ipynb) |

> หากเปิด Colab จากปุ่มด้านบน Notebook จะถูกเปิดโดยตรงจาก branch `main` ของ repository นี้

---

# Course Objectives

เมื่อเรียนครบชุด Notebook นี้ นิสิตควรสามารถ:

1. อธิบายโครงสร้างและตัวแปรของ radiosonde sounding ได้
2. ดาวน์โหลดและจัดเตรียมข้อมูลสำหรับการวิเคราะห์อย่างเป็นระบบ
3. ตรวจสอบคุณภาพข้อมูล (**Quality Control: QC**) ได้
4. วิเคราะห์ vertical atmospheric profile ได้
5. สร้างและอ่าน **Skew-T Log-P** ได้
6. คำนวณและตีความตัวแปรอุณหพลศาสตร์พื้นฐานของบรรยากาศได้
7. วิเคราะห์ atmospheric stability และ temperature inversion ได้
8. วิเคราะห์ลมชั้นล่างและศักยภาพการระบายตัวของบรรยากาศได้
9. ประมาณ **morning mixing-height diagnostic** ด้วย Bulk Richardson Number ได้
10. คำนวณ ventilation coefficient ได้
11. วิเคราะห์ความแปรผันตามเวลาและเปรียบเทียบ March–April 2024 ได้
12. ใช้ descriptive และ inferential statistics อย่างเหมาะสม
13. อธิบายข้อจำกัดของ statistical significance, correlation และ temporal autocorrelation ได้
14. เตรียมข้อมูล sounding สำหรับการนำไปวิเคราะห์ร่วมกับ PM₂.₅ ในขั้นต่อไปได้

---

# Teaching Dataset

## Station and Period

| Item | Information |
|---|---|
| Station | **Chiang Mai** |
| WMO station ID | **48327** |
| Study period | **1 March–30 April 2024** |
| Launch time used | **00 UTC** |
| Local time | **07:00 ICT** |
| March 2024 | **31 soundings** |
| April 2024 | **30 soundings** |
| Total | **61 soundings** |
| 00 UTC availability | **100%** |
| Data source | University of Wyoming Atmospheric Science Radiosonde Archive |

ข้อมูลถูกจัดเตรียมเป็น **common teaching dataset** เพื่อให้นิสิตทุกคนใช้ข้อมูลชุดเดียวกันและสามารถเปรียบเทียบผลการวิเคราะห์ได้

---

# Important Time Interpretation

ประเทศไทยใช้เวลา UTC+7 ดังนั้น

$$
00\ UTC = 07{:}00\ ICT
$$

ข้อมูลใน repository นี้จึงสะท้อน **morning atmospheric environment**

ไม่ควรตีความว่าเป็น:

- daily maximum CAPE
- daily maximum boundary-layer height
- atmospheric state ของทั้งวัน
- pollutant concentration

โดยเฉพาะค่าที่ประมาณจาก Bulk Richardson Number ควรเรียกว่า

> **morning mixing-height diagnostic**

ไม่ควรเรียกว่า daily maximum PBL height โดยอัตโนมัติ

---

# Sounding Data Structure

Radiosonde เป็นข้อมูลบรรยากาศในแนวดิ่ง

```text
1 sounding ≠ 1 row
1 sounding = many vertical atmospheric levels
```

ตัวแปรหลักใน teaching dataset ได้แก่:

| Variable | Meaning | Unit |
|---|---|---|
| `pressure_hPa` | Atmospheric pressure | hPa |
| `height_m` | Geopotential height | m |
| `temperature_C` | Air temperature | °C |
| `dewpoint_C` | Dew-point temperature | °C |
| `relative_humidity_pct` | Relative humidity | % |
| `mixing_ratio_gkg` | Water-vapor mixing ratio | g kg⁻¹ |
| `wind_direction_deg` | Meteorological wind direction | degree |
| `wind_speed_ms` | Wind speed | m s⁻¹ |
| `theta_K` | Potential temperature | K |
| `theta_e_K` | Equivalent potential temperature | K |
| `theta_v_K` | Virtual potential temperature | K |

---

# Repository Data Files

### `chiangmai_48327_sounding_20240301_20240430.csv.gz`

Combined radiosonde dataset สำหรับ March–April 2024

อ่านได้โดยตรงด้วย:

```python
import pandas as pd

df = pd.read_csv(
    "chiangmai_48327_sounding_20240301_20240430.csv.gz",
    compression="gzip"
)
```

### `launch_manifest_48327_20240301_20240430.csv`

ใช้ตรวจ:

- requested launch
- UTC hour
- download status
- data availability
- number of vertical levels
- structural QC เบื้องต้น

### `station_metadata_48327.csv`

Metadata ของสถานีและชุดข้อมูล

### `download_report_48327_20240301_20240430.txt`

รายงานสรุปการดาวน์โหลดและ data availability

### `README_DATASET.md`

รายละเอียดเฉพาะของ teaching dataset

---

# Notebook 00 — Data Preparation from University of Wyoming

**File:** `00_download_ChiangMai_48327_Sounding_MarApr2024.ipynb`

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/00_download_ChiangMai_48327_Sounding_MarApr2024.ipynb)

**สำหรับผู้สอน / ผู้ที่ต้องการเรียนรู้การสร้าง dataset**

Notebook นี้ใช้สำหรับ:

- ติดต่อ University of Wyoming radiosonde archive
- ดาวน์โหลด sounding
- ตรวจ availability ของเวลา synoptic ที่ร้องขอ
- เก็บ raw data และ processed data
- fixed-width parsing
- structural QC
- สร้าง launch manifest
- รวม sounding เป็น combined dataset
- สร้าง station metadata
- สร้าง download report
- เตรียมไฟล์สำหรับ GitHub

สำหรับการเรียนในชั้นเรียนทั่วไป นิสิต **ไม่จำเป็นต้องรัน Notebook 00**

---

# Notebook 01 — Get Teaching Dataset from GitHub

**File:** `01_get_ChiangMai_sounding_from_GitHub.ipynb`

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/01_get_ChiangMai_sounding_from_GitHub.ipynb)

**Notebook เริ่มต้นสำหรับนิสิต**

หน้าที่:

- mount Google Drive
- ดาวน์โหลด teaching files จาก GitHub
- ตรวจ station / time / date range
- ตรวจ data availability
- เลือก 00 UTC = 07:00 ICT
- ตรวจ March = 31 และ April = 30 soundings
- ทำ structural QC เบื้องต้น
- สร้าง analysis-ready teaching dataset
- บันทึก metadata และ QC outputs

ไฟล์หลักที่สร้าง:

```text
/content/drive/MyDrive/Teaching_Sounding_AirQuality/
01_data/chiangmai_48327_00UTC_20240301_20240430.csv.gz
```

Notebook 02–05 จะอ่านไฟล์นี้โดยตรง จึงไม่ต้องดาวน์โหลดข้อมูลจาก GitHub ใหม่ทุกบท

---

# Notebook 02 — Understanding Sounding Data and Quality Control

**File:** `02_understanding_sounding_data_and_QC.ipynb`

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/02_understanding_sounding_data_and_QC.ipynb)

หัวข้อสำคัญ:

- radiosonde sounding structure
- pressure และ geopotential height
- temperature และ dew point
- relative humidity
- mixing ratio
- wind speed / wind direction
- potential temperature
- equivalent potential temperature
- missing data
- structural quality control
- vertical profiles
- lower-tropospheric structure
- comparison of multiple sounding days

หลักการสำคัญ:

> **ก่อนคำนวณ diagnostic ทางอุตุนิยมวิทยา ต้องเข้าใจข้อมูลจริงและตรวจ QC ก่อนเสมอ**

---

# Notebook 03 — Skew-T and Atmospheric Thermodynamics

**File:** `03_SkewT_and_atmospheric_thermodynamics.ipynb`

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/03_SkewT_and_atmospheric_thermodynamics.ipynb)

ใช้ **MetPy** เพื่อศึกษา:

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
- wind barbs

Notebook ยังสร้าง daily thermodynamic diagnostics สำหรับ sounding ทั้ง 61 วัน

ตัวอย่าง output:

```text
03_daily_thermodynamic_diagnostics_48327_00UTC_MarApr2024.csv
```

### Important interpretation

ค่าจาก 07:00 ICT เป็น **morning thermodynamic environment**

CAPE/CIN ไม่ควรถูกตีความเป็น daily maximum instability โดยอัตโนมัติ

---

# Notebook 04 — Stability, Inversion and Air-Pollution Meteorology

**File:** `04_stability_inversion_and_air_pollution.ipynb`

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/04_stability_inversion_and_air_pollution.ipynb)

บทนี้เชื่อม atmospheric sounding กับ **air-quality meteorology**

หัวข้อ:

- Mean Sea Level (MSL) และ Above Ground Level (AGL)
- environmental lapse rate
- potential-temperature gradient
- static stability
- temperature inversion
- inversion base / top / depth
- inversion temperature difference
- inversion strength
- Brunt–Väisälä frequency \(N^2\)
- wind components \(u,v\)
- mean wind 0–500 m
- mean wind 0–1 km
- Bulk Richardson Number
- morning mixing-height diagnostic
- ventilation coefficient

### Temperature Inversion

Temperature inversion คือชั้นที่:

$$
\frac{dT}{dz} > 0
$$

Notebook ใช้ teaching thresholds เช่น:

```text
minimum inversion depth = 100 m
minimum ΔT              = 0.5 °C
```

ค่าดังกล่าวเป็น methodological choices สำหรับการเรียน ไม่ใช่มาตรฐานสากลตายตัว

### Bulk Richardson Number

แนวคิดโดยทั่วไป:

$$
Ri_b =
\frac{
(g/\theta_{v0})
[\theta_v(z)-\theta_{v0}]z
}{
[u(z)-u_0]^2+[v(z)-v_0]^2
}
$$

ใช้ \(Ri_b \approx 0.25\) เป็น teaching threshold สำหรับประมาณ morning mixing height

### Ventilation Coefficient

$$
VC = MH \times \overline{U}
$$

โดย:

- \(MH\) = mixing-height diagnostic
- \(\overline{U}\) = mean wind ภายใน mixing layer

ตัวอย่าง output:

```text
04_daily_airpollution_meteorology_48327_00UTC_MarApr2024.csv
```

---

# Notebook 05 — Temporal and Statistical Analysis

**File:** `05_temporal_statistics_MarApr2024.ipynb`

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/05_temporal_statistics_MarApr2024.ipynb)

Notebook นี้รวมผลจาก Notebook 03 และ 04 เป็น daily analysis table จำนวน 61 วัน

### Descriptive statistics

- mean
- standard deviation
- median
- IQR
- P10 / P25 / P50 / P75 / P90
- skewness

### Temporal analysis

- daily time series
- 3-day rolling mean
- 7-day rolling mean

### March vs April 2024

- boxplot
- ECDF
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
- simple autocorrelation function
- approximate effective sample size

Merged daily dataset:

```text
05_merged_daily_sounding_diagnostics_48327_MarApr2024.csv
```

ไฟล์นี้เป็นจุดเริ่มต้นที่เหมาะสำหรับนำไปวิเคราะห์ร่วมกับ PM₂.₅ ในขั้นต่อไป

---

# Quick Student Workflow

สำหรับการเรียนปกติ:

1. เปิดและรัน Notebook 01
2. ตรวจว่า teaching dataset ถูกบันทึกใน Google Drive
3. รัน Notebook 02 → 03 → 04 → 05 ตามลำดับ
4. อ่าน Markdown explanation ก่อนรันแต่ละ code cell
5. ทำ Exercise ตอนท้ายของแต่ละบท
6. เก็บ CSV outputs และ figures เพื่อใช้ในบทถัดไป

เริ่ม Notebook 01:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/nattaponm/Teaching_Sounding_AirQuality/blob/main/01_get_ChiangMai_sounding_from_GitHub.ipynb)

---

# Google Drive Folder Structure

หลังจากรัน Notebook 01:

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

Core libraries:

- Python
- pandas
- NumPy
- Matplotlib
- SciPy
- MetPy
- requests

นิสิตไม่จำเป็นต้องติดตั้ง Python บนเครื่องส่วนตัวหากใช้ Google Colab

---

# Quality-Control Philosophy

ชุดการสอนใช้หลัก:

> **QC flag first; remove or modify data only with scientific justification.**

ตัวอย่าง QC:

- missing values
- duplicated pressure levels
- pressure ordering
- height ordering
- \(T_d \le T\)
- RH range
- vertical profile completeness
- derived-variable missingness

ไม่ควรใช้ `dropna()` กับทุกตัวแปรทั้ง sounding โดยไม่มีเหตุผล เพราะอาจทำให้สูญเสียข้อมูลที่ยังใช้ประโยชน์ได้

---

# Scientific Interpretation for Air Quality

## Atmospheric Stability

Potential temperature ช่วยอธิบาย static stability

โดยทั่วไป:

$$
\frac{d\theta}{dz} > 0
$$

สัมพันธ์กับ stable stratification

---

## Temperature Inversion and PM₂.₅

การพบ temperature inversion **ไม่ได้หมายความโดยอัตโนมัติว่า PM₂.₅ ต้องสูง**

PM₂.₅ ยังขึ้นกับ:

- emission sources
- biomass burning
- regional transport
- wind
- atmospheric chemistry
- relative humidity
- precipitation
- deposition
- boundary-layer evolution

ดังนั้น sounding ใช้บอก:

> **meteorological conditions favorable or unfavorable for pollutant dispersion**

ไม่ใช่ใช้แทน pollutant observations

---

## Mixing Height

Bulk-Richardson-based mixing height ในชุดนี้เป็น:

> **morning mixing-height diagnostic**

เนื่องจาก sounding อยู่ที่ประมาณ 07:00 ICT

ค่าดังกล่าวไม่ควรใช้แทน daily maximum daytime PBL height โดยไม่มีข้อมูลเพิ่มเติม

---

## Ventilation Coefficient

Ventilation coefficient เป็น atmospheric dispersion diagnostic

ไม่ใช่ pollutant concentration

ดังนั้น:

```text
low ventilation
        ↓
meteorologically less favorable for dispersion
```

ไม่เท่ากับ:

```text
low ventilation
        ↓
PM2.5 must be high
```

---

# Statistical Interpretation

## Statistical Significance ≠ Physical Significance

ถ้า:

$$
p < 0.05
$$

ไม่ได้หมายความโดยอัตโนมัติว่า:

- effect มีขนาดใหญ่
- มีความสำคัญทางกายภาพเสมอ
- causal relationship ถูกพิสูจน์แล้ว
- \(H_0\) มีโอกาสเป็นจริง 5%

ควรพิจารณาร่วมกับ:

- descriptive statistics
- distributions
- effect size
- sample size
- atmospheric physics

---

## Correlation ≠ Causation

$$
correlation \neq causation
$$

ตัวอย่างเช่น ventilation coefficient มี mixing height อยู่ในสมการ ดังนั้น correlation ระหว่างสองตัวแปรนี้อาจเกิดบางส่วนจาก **mathematical coupling**

---

## Temporal Autocorrelation

ข้อมูลบรรยากาศรายวันอาจมี persistence:

$$
X_t \not\perp X_{t-1}
$$

ดังนั้น 61 daily observations ไม่จำเป็นต้องเท่ากับ 61 independent pieces of information

Notebook 05 จึงแนะนำแนวคิด:

- lag-1 autocorrelation
- ACF
- approximate effective sample size

เพื่อให้นิสิตเข้าใจข้อจำกัดของ independent-sample statistical tests

---

# March–April 2024 Interpretation

การเปรียบเทียบในชุดนี้ควรเรียกว่า:

> **March–April comparison within 2024**

ไม่ควรเรียกว่า climatological seasonal comparison เพราะใช้ข้อมูลเพียงหนึ่งปี

```text
March 2024 : n = 31
April 2024 : n = 30
Total      : n = 61
```

---

# Suggested Learning Sequence

| Level | Notebook | Main learning outcome |
|---|---|---|
| **Level 1 — Data** | 01–02 | Dataset, QC, vertical profiles |
| **Level 2 — Atmospheric Physics** | 03 | Skew-T, parcel theory, thermodynamics |
| **Level 3 — Air-Quality Meteorology** | 04 | Stability, inversion, mixing, ventilation |
| **Level 4 — Data Analysis** | 05 | Temporal variability and statistics |

---

# Exercises

แต่ละ Notebook มี Exercise เพื่อให้นิสิต:

- เปลี่ยนวันที่
- เปรียบเทียบ sounding profiles
- เปลี่ยน parameter
- ทดสอบ inversion thresholds
- ทดสอบ Bulk Richardson threshold
- เปรียบเทียบ CAPE definitions
- เปรียบเทียบ March–April
- วิเคราะห์ correlation
- วิเคราะห์ temporal autocorrelation
- อธิบายข้อจำกัดของผลลัพธ์

เป้าหมายคือให้นิสิตตอบได้ว่า:

> **“ทำไมจึงใช้วิธีนี้ และผลลัพธ์มีความหมายทางกายภาพอย่างไร?”**

---

# Future Extension — PM₂.₅ Mini Research Project

ไฟล์:

```text
05_merged_daily_sounding_diagnostics_48327_MarApr2024.csv
```

สามารถนำไปรวมกับข้อมูล PM₂.₅ เพื่อศึกษา exploratory relationships เช่น:

$$
PM_{2.5} \leftrightarrow Mixing\ Height
$$

$$
PM_{2.5} \leftrightarrow Inversion\ Strength
$$

$$
PM_{2.5} \leftrightarrow Ventilation\ Coefficient
$$

$$
PM_{2.5} \leftrightarrow PWAT
$$

$$
PM_{2.5} \leftrightarrow Low\ Level\ Wind
$$

ข้อมูลที่สามารถเพิ่มในขั้นต่อไป:

- Air4Thai PM₂.₅
- hotspot / biomass burning
- surface meteorology
- precipitation
- ERA5
- back trajectories
- emission information

สิ่งสำคัญคือ sounding เป็นเพียง **meteorological component** ของระบบ air pollution

---

# Reproducibility

Repository นี้ออกแบบให้ทุกคนใช้ teaching dataset เดียวกัน:

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

แนวทางนี้ช่วย:

- ตรวจสอบขั้นตอนย้อนหลัง
- ทำซ้ำผลลัพธ์ได้
- ลดความแตกต่างจากการดาวน์โหลดข้อมูลคนละช่วงเวลา
- แยก data acquisition ออกจาก scientific analysis
- รักษา output ของแต่ละขั้นให้ตรวจสอบได้

---

# Scientific Caveats

1. Sounding เป็น snapshot ของเวลาหนึ่ง ไม่ใช่สภาวะของทั้งวัน
2. 00 UTC ที่เชียงใหม่คือประมาณ 07:00 ICT
3. CAPE ตอนเช้าไม่ใช่ daily maximum CAPE
4. Ri-based mixing height เป็น diagnostic estimate
5. inversion detection ขึ้นกับ algorithm และ thresholds
6. ventilation coefficient ไม่ใช่ตัววัด PM₂.₅
7. correlation ไม่แสดง causation
8. p-value ไม่แสดงขนาดของผล
9. temporal autocorrelation ควรถูกพิจารณาใน daily data
10. March–April 2024 ไม่ใช่ climatology
11. atmospheric diagnostics ควรถูกตีความร่วมกับ emission, transport และ pollutant observations

---

# Data Source

Radiosonde observations:

**University of Wyoming Atmospheric Science Radiosonde Archive**

ข้อมูลถูกดาวน์โหลดและจัดเตรียมเป็น compact teaching dataset สำหรับการเรียนการสอนและการวิเคราะห์ทางวิชาการ

เมื่อใช้ข้อมูลหรือพัฒนางานวิจัยต่อ ควรระบุ data source, station, observation time และ study period ให้ชัดเจน

---

# Repository

**Teaching_Sounding_AirQuality**

https://github.com/nattaponm/Teaching_Sounding_AirQuality

---

# Citation / Acknowledgment

หากนำ Notebook หรือ workflow นี้ไปใช้ในการเรียน การสอน หรือพัฒนางานวิจัยต่อ ควรอ้างถึง repository และแหล่งข้อมูลต้นทางตามความเหมาะสม

ตัวอย่าง acknowledgement:

> Radiosonde data used in this exercise were obtained from the University of Wyoming Atmospheric Science Radiosonde Archive. Data processing and teaching workflows were implemented in Python using open-source scientific libraries.

---

# For Students

ไม่จำเป็นต้องจำคำสั่ง Python ทุกคำสั่ง

สิ่งสำคัญกว่าคือสามารถอธิบายได้ว่า:

1. ตัวแปรแต่ละตัวหมายถึงอะไร
2. ทำไมต้อง QC
3. atmospheric profile บอกอะไร
4. stability และ inversion มีผลต่อ vertical mixing อย่างไร
5. wind มีผลต่อ atmospheric dispersion อย่างไร
6. mixing-height diagnostic มีข้อจำกัดอะไร
7. สถิติที่ใช้ตอบคำถามอะไร
8. ข้อจำกัดของข้อมูลและวิธีวิเคราะห์คืออะไร

เป้าหมายสุดท้ายคือการเปลี่ยนจาก:

```text
"รันโค้ดได้"
```

ไปสู่:

```text
"เข้าใจข้อมูล → เข้าใจฟิสิกส์ → วิเคราะห์ได้ → ตีความได้"
```
