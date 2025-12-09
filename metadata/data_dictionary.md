# Data Dictionary

This project uses processed climate and crop datasets derived from NOAA GHCND and USDA NASS QuickStats. Column definitions, units, and sources are listed below.

## Processed climate data (`data/climate_data/processed/ghcnd-IA-data.csv`)
Source: NOAA Global Historical Climatology Network Daily (GHCND) Iowa stations aggregated to annual means for June–August. Daily TMAX/TMIN values are averaged per month (ignoring `-9999`), then averaged across stations per year.

| Column | Type | Units | Description |
| --- | --- | --- | --- |
| YEAR | integer | year | Calendar year (YYYY). |
| Average Temperature | float | degrees Celsius | Annual mean of station monthly means for TAVG (computed as (TMAX+TMIN)/2). |
| Average TMAX | float | degrees Celsius | Annual mean of station monthly mean TMAX values. |
| Average TMIN | float | degrees Celsius | Annual mean of station monthly mean TMIN values. |

## Processed crop data (`data/crop_data/processed/crop.csv`)
Source: USDA NASS QuickStats corn grain yield for Iowa; cleaned to a single record per year.

| Column | Type | Units | Description |
| --- | --- | --- | --- |
| YEAR | integer | year | Calendar year (YYYY). |
| Yield | float | bushels per acre | Annual corn grain yield for Iowa (QuickStats). |

## Merged climate and crop data (`data/merged/crop_yield_and_temperature.csv`)
Source: Result of `merge_crop_and_temp_data(crop_df, temp_df)` in `climate_crop_study.ipynb`, joining processed crop and climate datasets on `YEAR`.

| Column | Type | Units | Description |
| --- | --- | --- | --- |
| YEAR | integer | year | Calendar year (YYYY). |
| Yield | float | bushels per acre | Annual corn grain yield for Iowa. |
| Average Temperature | float | degrees Celsius | Annual mean temperature across Iowa GHCND stations. |

## Raw crop data (`data/crop_data/raw_data/corn.csv`)
Source: USDA NASS QuickStats CSV export (filters commonly include State=IOWA, Commodity=CORN, Data Item=CORN, GRAIN - YIELD, Program=SURVEY). Some fields may be empty for certain records.

| Column | Type | Units | Description |
| --- | --- | --- | --- |
| Program | string | n/a | Data collection program (e.g., SURVEY). |
| Year | integer | year | Year of observation or forecast. |
| Period | string | n/a | Time period label (e.g., YEAR, YEAR - AUG FORECAST). |
| Week Ending | string | date | Week ending date when relevant; blank for annual data. |
| Geo Level | string | n/a | Geographic aggregation level (e.g., STATE). |
| State | string | n/a | State name (IOWA). |
| State ANSI | string | n/a | State ANSI code. |
| Ag District | string | n/a | Agricultural statistics district name. |
| Ag District Code | string | n/a | District code. |
| County | string | n/a | County name if provided. |
| County ANSI | string | n/a | County ANSI code. |
| Zip Code | string | n/a | ZIP code if provided. |
| Region | string | n/a | Region descriptor if provided. |
| watershed_code | string | n/a | Watershed code if provided. |
| Watershed | string | n/a | Watershed name if provided. |
| Commodity | string | n/a | Commodity name (CORN). |
| Data Item | string | n/a | Data item descriptor (e.g., CORN, GRAIN - YIELD, MEASURED IN BU / ACRE). |
| Domain | string | n/a | Domain (e.g., TOTAL). |
| Domain Category | string | n/a | Domain category detail (e.g., NOT SPECIFIED). |
| Value | float | bushels per acre | Reported measurement for the record (yield). |
| CV (%) | float | percent | Coefficient of variation (data quality indicator) when supplied. |

## Climate source data
NOAA GHCND daily station files and station metadata are the upstream sources for the processed climate data. Retrieve current data from NOAA NCEI (https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily) or download it while re-running the processing steps documented in `metadata/provenance.md` to regenerate `ghcnd-IA-data.csv`.

### GHCND `.dly` column ranges (from `data/climate_data/raw_data/readme.txt`)
Each record is a month for one station; daily values repeat in blocks (VALUE#, MFLAG#, QFLAG#, SFLAG# for days 1–31).

| Variable | Columns | Type | Notes |
| --- | --- | --- | --- |
| ID | 1-11 | character | Station identifier. |
| YEAR | 12-15 | integer | Year of record. |
| MONTH | 16-17 | integer | Month of record. |
| ELEMENT | 18-21 | character | Measurement code (e.g., TMAX, TMIN, PRCP, SNOW, SNWD). |
| VALUE1 | 22-26 | integer | Day 1 value (units depend on ELEMENT; temps are tenths °C). |
| MFLAG1 | 27 | character | Measurement flag for day 1. |
| QFLAG1 | 28 | character | Quality flag for day 1. |
| SFLAG1 | 29 | character | Source flag for day 1. |
| VALUE2 | 30-34 | integer | Day 2 value (pattern repeats through day 31). |
| ... | ... | ... | Continues as VALUE#, MFLAG#, QFLAG#, SFLAG# up to day 31 (VALUE31 at cols 262-266, flags at 267-269). |
