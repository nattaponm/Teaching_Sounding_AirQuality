# Chiang Mai Radiosonde Teaching Dataset

## Dataset

- Station: Chiang Mai
- WMO ID: 48327
- Period: 2024-03-01 to 2024-04-30
- UTC hours checked: [0, 6, 12]
- Local timezone: ICT (UTC+7)
- Source: University of Wyoming Atmospheric Science Radiosonde Archive

## Availability

```text
 hour_utc  hour_ict  requested  success  no_data  error  availability_pct
        0         7         61       61        0      0             100.0
        6        13         61        0        0     61               0.0
       12        19         61        0        0     61               0.0
```

## Recommended file for student notebooks

`chiangmai_48327_sounding_20240301_20240430.csv.gz`

Pandas can read the compressed CSV directly:

```python
import pandas as pd
df = pd.read_csv("RAW_GITHUB_URL_HERE", compression="gzip")
```

## Provenance files

- `launch_manifest_48327_20240301_20240430.csv`
- `station_metadata_48327.csv`
- `download_report_48327_20240301_20240430.txt`
- `chiangmai_48327_sounding_20240301_20240430_raw_profiles.zip`

Data source: University of Wyoming Atmospheric Science Radiosonde Archive.
Retain source attribution and review current source data-use conditions before public redistribution.