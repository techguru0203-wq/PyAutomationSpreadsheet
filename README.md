# PyAutomationSpreadsheet

## A Python automation tool that processes local CSV reports, applies transformation logic (date normalization, spend/margin calculations, etc.), and updates a Google Spreadsheet automatically.
## Useful for marketing teams, analysts, and operations workflows that require synchronized reporting between CSV exports and Google Sheets.
---
###📌 Features

Reads and validates CSV report files

Converts date formats (local CSV → Google Sheet format)

Matches CSV rows with Google Sheet rows by date & campaign

Applies business logic for margin calculations

Updates Google Sheets tabs (Daily & Monthly)

Outputs transformed CSV files locally

---

### 🧰 Tech Stack

Python 3.6+

gspread + oauth2client — Google Sheets API

pandas — CSV parsing & data wrangling

python-dateutil — date parsing

Standard library utilities (argparse, json, datetime, etc.)

---

### ⚠️ Required Fixes / Setup Before Running

This project will NOT run until the following items are configured.
These are not “bugs” in the code — they are required environment setup steps.

###1️⃣ Install Dependencies

Create a virtual environment and install required packages:
```bash
python3 -m venv .venv
```
```bash
source .venv/bin/activate      # Windows: .venv\Scripts\activate
```

```bash
pip install -r requirements.txt
```

### 2️⃣ Add report_config.json

The app cannot run without this file.
Place it inside the project root:
```kotlin
GoogleSpreadSheetAutomation/
    app.py
    report_config.json   ← you must create this
```

#### ✔️ Example report_config.json

This is a working sample based on how the code reads configuration:

```json
{
  "google_credentials_file": "service_account.json",
  "spread_sheet": "https://docs.google.com/spreadsheets/d/YOUR_SHEET_ID/edit#gid=0",
  "reports": [
    {
      "REPORT_TYPE": "The Trade Desk",

      "csv_date_column": "Date",
      "campaign_col": "Campaign",
      "spend_col": "Spend",

      "daily_margin_column_name": "Daily Margin",
      "monthly_margin_column_name": "Monthly Margin",

      "csv_date_format": "%d-%m-%Y"
    }
  ]
}
```
### ⚠️ Fix you MUST apply

Keys in this file must match exact variable names in constants.py, e.g.

spend_col (NOT csv_spend_col)

csv_date_format must be a valid Python strftime string

Your CSV headers MUST match this config exactly.


### 3️⃣ Add Google Service Account Credentials

To allow API access:

Go to Google Cloud Console

Create new project

Enable:

Google Sheets API

Google Drive API

Create Service Account

Generate JSON key → rename to ```service_account.json```

Place it next to ```app.py```

Share spreadsheet with the service account email

### 4️⃣ Prepare Input CSV Files

The script expects:

A folder containing one CSV file

CSV must contain all configured columns

CSV dates must match the configured date format (e.g. %d-%m-%Y → 01-01-2020)

Example structure:
```lua
input/
    report.csv
output/
```

### ▶️ Running the Application
Real Google Sheets automa/tion:
```bash
python app.py --input ./input --output ./output --report_type "The Trade Desk"
```

If everything is configured correctly, the app will:

Read your local CSV

Fetch the Google Sheet worksheets

Match rows based on date & campaign

Apply calculations

Write processed CSVs to /output/

Update Google Sheets tabs

---

## 🧪 Optional: Local Test Mode (No Google Required)

If you want to test logic without API access, internet, or credentials, use the included ```mock_run.py``` file (created during debugging).

This script:

Bypasses Google Sheets

Uses mock worksheet data

Verifies CSV processing logic end-to-end


Run:

```bash
python mock_run.py
```


It will produce output in the ```output/``` folder.

---
```text
GoogleSpreadSheetAutomation/
│
├── app.py
├── constants.py
├── utils.py
├── report_config.json         ← YOU create this
├── service_account.json       ← YOU provide this
│
├── cloud_module/
│   └── cloud_operations.py
│
├── local_module/
│   └── local_operations.py
│
├── input/
│   └── report.csv
│
├── output/
│   └── (generated files)
│
└── mock_run.py                ← Testing without Google API
```
---

### ❗ Common Errors & Fixes
❌ ModuleNotFoundError: gspread

You didn’t install requirements:

```bash
pip install -r requirements.txt
```


#### ❌ Please make sure that report_config.json file exists

You didn’t create report_config.json.

#### ❌ Error while reading Column with key X

Your CSV headers don’t match your config.

#### ❌ RowNotMatchedException

The script cannot match CSV rows to Google Sheet rows.
Check:

-date format in CSV

-campaign names

-margin column names

-worksheet data exists in Google Sheet
