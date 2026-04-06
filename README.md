# Data Processing Pipeline: CSV to JSON Transformation

This project demonstrates a declarative data pipeline using a YAML configuration to ingest, filter, and export transaction data.

## 1. Project Overview
The pipeline performs an ETL (Extract, Transform, Load) process on a local dataset. It reads transaction records, applies a Python-based filter to isolate high-value transactions, and outputs the result in JSON format.

## 2. Setup & Dataset
Create a local file named `transactions.csv` in your environment (e.g., `/home/gauravjainn/`).

### Input Data (`transactions.csv`)
| transaction_id | item | category | amount |
| :--- | :--- | :--- | :--- |
| T001 | Laptop | Electronics | 1200 |
| T002 | T-Shirt | Apparel | 25 |
| T003 | Keyboard | Electronics | 75 |
| T004 | Coffee Maker | Home | 100 |
| T005 | Mouse | Electronics | 20 |

---

## 3. Pipeline Configuration
The pipeline logic is defined in the following configuration. It uses a **Filter** transform to only keep records where the amount is greater than 100.

```yaml
pipeline:
  transforms:
    - type: ReadFromCsv
      config:
        path: /home/gauravjainn/transactions.csv

    - type: Filter
      config:
        language: python
        # Keeping rows where amount > 100
        keep: "amount > 100"
      input: ReadFromCsv

    - type: WriteToJson
      config:
        path: /home/gauravjainn/transaction.json
      input: Filter
