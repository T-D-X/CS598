# Provenance and Processing

This document summarizes how raw data are obtained and transformed into the processed datasets used in `climate_crop_study.ipynb`.

## Sources
- Climate: NOAA Global Historical Climatology Network Daily (GHCND) via NCEI (https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily). Station-level daily observations for Iowa.
- Crop: USDA NASS QuickStats (https://quickstats.nass.usda.gov/), corn grain yield for Iowa.

## Processing steps
### Climate data (`data/climate_data/processed/ghcnd-IA-data.csv`)
- Retrieve Iowa GHCND station data (daily observations).
- Filter to Iowa stations and TMAX/TMIN.
- Parse daily values per month, drop missing sentinel `-9999`, and compute monthly means in °C (daily tenths °C ÷ 10).
- Compute station-level TMAX/TMIN monthly means, then TAVG = (TMAX + TMIN) / 2.
- Aggregate across stations to annual means for each metric and round to 1 decimal place.
- Save columns: `YEAR`, `Average Temperature`, `Average TMAX`, `Average TMIN`.

### Crop data (`data/crop_data/raw_data/corn.csv` -> `data/crop_data/processed/crop.csv`)
- Download QuickStats CSV for corn with filters such as: State=IOWA, Commodity=CORN, Data Item=CORN, GRAIN - YIELD, Program=SURVEY.
- Keep records representing annual yield (e.g., `Period` = YEAR) and drop forecast/duplicate period rows when rolling up.
- Convert `Value` to numeric, retain yield in bushels per acre, and select relevant columns.
- Save processed output as two-column CSV: `YEAR`, `Yield`.

### Merged climate and crop data (`data/merged/crop_yield_and_temperature.csv`)
- Merge processed climate and crop datasets on `YEAR` using `merge_crop_and_temp_data(crop_df, temp_df)` in `climate_crop_study.ipynb`.
- Output columns: `YEAR`, `Yield` (bushels per acre), `Average Temperature` (°C).

## Reproduction
- Environment: install dependencies from `requirements.txt` (Python 3.13).
- Notebook: `climate_crop_study.ipynb` contains the loading, cleaning, merge, and visualization steps. Run cells sequentially after placing raw data in `data/crop_data/raw_data` (and, if regenerating climate data, the NOAA GHCND station/day files).
- If raw climate files are refreshed, re-run the climate aggregation portion to regenerate `ghcnd-IA-data.csv`.

## Notes
- Document download dates and exact API queries when refreshing data to preserve lineage.
- Update `metadata/dataset.jsonld` and this file whenever processing logic or sources change.

## Data citations
- NOAA NCEI GHCN-Daily: Menne, M.J., I. Durre, R.S. Vose, B.E. Gleason, and T.G. Houston, 2012: Global Historical Climatology Network - Daily (GHCN-Daily), Version 3. NOAA National Climatic Data Center. DOI:10.7289/V5D21VHZ. Accessed <date>.
- USDA National Agricultural Statistics Service (QuickStats): USDA NASS QuickStats Database. Accessed <date>. Include the date the corn yield data were retrieved.
