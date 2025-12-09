# CS598 Climate & Crop Study

End-to-end data curation workflow for exploring how climate trends relate to crop yields in Iowa.

## About this project
This project is mainly contained in `climate_crop_study.ipynb` and does the following:
1. Downloadas and filters NOAA Global Historical Climatology Network Daily (GHCND) data for Iowa and aggregates annual average temperatures across all weather stations.
2. Cleans USDA NASS QuickStats corn yield data for Iowa and aligns it with the climate time series.
3. Join the datasets and visualize temperature vs. yield to explore potential relationships and trends.

## System requirements
- Python 3.13 (tested with 3.13.x)

## Setup and running `climate_crop_study.ipynb`
1. Create a virtual environment with Python 3.13 and activate it:
   ```bash
   python3.13 -m venv .venv
   source .venv/bin/activate
   ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
3. Launch Jupyter and open the notebook:
   ```bash
   jupyter notebook climate_crop_study.ipynb
   ```
4. Run the notebook cells in order. The data paths referenced inside the notebook assume the directory layout shown below.

## Data locations and downloads
- Climate data
  - Raw: `data/climate_data/raw_data/ghcnd-stations.txt` (station metadata from NOAA GHCND).
  - Processed: `data/climate_data/processed/ghcnd-IA-data.csv` (annual averages for Iowa with columns: `YEAR`, `Average Temperature`, `Average TMAX`, `Average TMIN`; computed from daily TMAX/TMIN, ignoring `-9999`).
  - To refresh: download Iowa station/daily data from NOAA GHCND (https://www.ncei.noaa.gov/products/land-based-station/global-historical-climatology-network-daily), filter to Iowa stations, or delete the existing data from this repository before running the notebook.
- Crop data
  - Raw: `data/crop_data/raw_data/corn.csv` (USDA NASS QuickStats export for Iowa corn).
  - Processed: `data/crop_data/processed/crop.csv` (cleaned yearly yields).
  - To refresh: request an updated CSV from USDA NASS QuickStats (https://quickstats.nass.usda.gov/) using filters such as State = IOWA, Commodity = CORN, Data Item = CORN, GRAIN - YIELD, Program = SURVEY.

## Data citations
- NOAA NCEI GHCN-Daily: Menne, M.J., I. Durre, R.S. Vose, B.E. Gleason, and T.G. Houston, 2012: Global Historical Climatology Network - Daily (GHCN-Daily), Version 3. NOAA National Climatic Data Center. DOI:10.7289/V5D21VHZ. Accessed <date>.
- USDA National Agricultural Statistics Service (QuickStats): USDA NASS QuickStats Database. Accessed <date>. Include the access date you retrieved the corn yield data.
