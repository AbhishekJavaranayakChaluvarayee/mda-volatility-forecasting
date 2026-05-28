# Data Requirements

This project requires two proprietary data sources that cannot be shared
publicly due to licensing restrictions.

## 1. CRSP Daily Stock Data
Source: Center for Research in Security Prices via Wharton Research Data
Services (WRDS)
Website: https://wrds-www.wharton.upenn.edu/
Requires: WRDS subscription (available through most universities)

Variables required: PERMNO, date, ticker, PRC, RET, CFACPR
Date range: 1 January 2019 to 31 December 2024
Save as: data/raw/crsp_daily.csv

## 2. SEC EDGAR MD&A Text Files
Source: SEC EDGAR (free, publicly accessible)
Website: https://www.sec.gov/cgi-bin/browse-edgar
API: https://data.sec.gov/submissions/

The extraction pipeline in notebooks/02_mda_extraction/ downloads and
processes these files automatically.

## 3. Firm List
A sample of the firm list format is provided in data/sample/firm_list_sample.csv
The full firm list of 148 S&P 500 large-cap firms is not included but can
be constructed from any S&P 500 constituent list for the study period.

## Pipeline Order
Run notebooks in this order after setting up your data:
1. notebooks/01_firm_universe/02_build132firms.ipynb
2. notebooks/02_mda_extraction/ (run all six in order)
3. notebooks/03_price_data/04_updatedprice.ipynb
4. notebooks/04_feature_engineering/ (run all five in order)
5. notebooks/05_modelling_development/ (run all three in order)
6. notebooks/06_final_modelling/12_dataset_v4.ipynb