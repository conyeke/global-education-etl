# Global Education & Development Data Pipeline

This Python script automates data extraction, transformation, and merging from multiple sources to produce a unified dataset containing country-level school enrollment data, regional development indicators, and country metadata.

## Overview

The pipeline follows an ETL (Extract → Transform → Load) process:

1. **Extract**
   - Fetches World Bank data (school enrollment rates by gender) via API.
   - Fetches UNDP Region Index data via API.
   - Reads a local `all_countries.csv` file containing country metadata.

2. **Transform**
   - Normalizes and flattens JSON structures from APIs.
   - Cleans and merges all datasets into a single DataFrame.
   - Performs full outer joins to preserve all records.

3. **Load**
   - Exports the final merged dataset to `final_merged_data.csv`.

## Data Sources

| Source | Description | Format | Access Method |
|--------|--------------|--------|----------------|
| World Bank API | Primary school enrollment, female (% of gross) for 2024 | JSON | REST API (`https://api.worldbank.org/v2/...`) |
| UNDP Region Index API | Regional groupings and development index metadata | JSON | REST API (`https://api.open.undp.org/api/region-index.json`) |
| Local CSV | Country metadata (including ISO3 codes) | CSV | Local file (`all_countries.csv`) |

## Project Structure

```
project_root/
├── all_countries.csv
├── school_enrollment.json
├── region_index.json
├── final_merged_data.csv
├── main.py
└── README.md
```

## Configuration

```python
LOCAL_CSV_PATH = 'all_countries.csv'
WB_JSON_PATH = 'school_enrollment.json'
UNDP_JSON_PATH = 'region_index.json'
OUTPUT_CSV_PATH = 'final_merged_data.csv'

WB_BASE_URL = 'https://api.worldbank.org/v2/country/all/indicator/SE.ENR.PRSC.FM.ZS?format=json&date=2024'
UNDP_URL = 'https://api.open.undp.org/api/region-index.json'
WB_PAGES_LIMIT = 6
```

## How to Run

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/global-education-etl.git
cd global-education-etl
```

### 2. Install dependencies
```bash
pip install pandas requests
```

### 3. Prepare input data
Ensure that `all_countries.csv` exists in the same directory.  
It must contain an `iso3` column.

Example:
```csv
iso3,country_name
USA,United States
FRA,France
BRA,Brazil
```

### 4. Run the script
```bash
python main.py
```

### 5. Output
A file named `final_merged_data.csv` will be created in the working directory.

## Function Overview

| Function | Description |
|-----------|-------------|
| `extract_data()` | Fetches data from World Bank and UNDP APIs and saves them as JSON. |
| `transform_and_merge_data(file_paths)` | Loads files, normalizes JSON, and performs full outer joins. |
| `load_data(df, output_path)` | Saves the final merged DataFrame to CSV. |

## Example Output

```
--- Starting Data Extraction Process ---
Fetched page 1 with 50 records.
Fetched page 2 with 50 records.
Saved 300 total records to school_enrollment.json
Successfully fetched UNDP data.
Saved records to region_index.json
--- Data Extraction Complete ---

--- Starting Data Transformation and Merge Process ---
Loaded all_countries_df: 195 rows.
Transformed region_index_df: 220 rows.
Transformed school_enrollment_df: 190 rows.
Data transformation and merging complete.

--- Data Load Complete ---
Successfully saved 195 rows to final_merged_data.csv
```

## Error Handling

- Network and parsing errors are caught and logged.
- Script continues gracefully when possible.
- Empty or failed transformations are skipped safely.

## Dependencies

- pandas  
- requests  
- json  
- time  
- os  
- functools  

## Extending the Script

- Modify `WB_BASE_URL` to fetch a different World Bank indicator.
- Add new APIs by extending the `extract_data()` and merge logic.
- Integrate with schedulers (cron, Airflow) for automation.

## License

This project is open-source under the MIT License.

## Author

Developed by Collins Agubuike — Data Engineering & Automation Enthusiast

Contact: nchillns@yahoo.com
