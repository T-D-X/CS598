# Data Dictionary

This project uses processed climate and crop datasets derived from NOAA GHCND and USDA NASS QuickStats. Column definitions, units, and sources are listed below.

## Processed climate data (`data/climate_data/processed/ghcnd-IA-data.csv`)
Source: NOAA Global Historical Climatology Network Daily (GHCND) Iowa stations aggregated to annual mean temperature.

| Column | Type | Units | Description |
| --- | --- | --- | --- |
| YEAR | integer | year | Calendar year (YYYY). |
| Average Temperature | float | degrees Celsius | Annual mean temperature across Iowa GHCND stations (mean of station daily averages rolled up to a statewide annual mean). |

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
| State ANSI | string | n/a | State FIPS/ANSI code. |
| Ag District | string | n/a | Agricultural statistics district name. |
| Ag District Code | string | n/a | District code. |
| County | string | n/a | County name if provided. |
| County ANSI | string | n/a | County FIPS/ANSI code. |
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

## Climate source data (not stored in repo)
NOAA GHCND daily station files and station metadata are the upstream sources for the processed climate data. Retrieve current data from NOAA NCEI (https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily) and re-run the processing steps documented in `metadata/provenance.md` to regenerate `ghcnd-IA-data.csv`.
