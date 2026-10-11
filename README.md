# Water Quality Medallion Pipeline (Azure Databricks)

End-to-end data engineering project on European water quality data
(EEA Waterbase WISE6 aggregated data) using the Medallion architecture
on Azure.

## Architecture

```
CSV -> ADLS Gen2 -> Bronze (raw Delta) -> Silver (cleaned) -> Gold (analytics-ready)
```

| Layer | Notebook | What it does |
|-------|----------|--------------|
| Bronze | `Project_Code/Project_Code/Code/Bronze_Layer.ipynb` | Reads raw CSV from storage and writes it as a Delta table |
| Silver | `Project_Code/Project_Code/Code/Silver_Layer.ipynb` | Drops unused columns, renames columns, maps country / water body / matrix codes to names |
| Gold | `Project_Code/Project_Code/Code/Gold_Layer.ipynb` | Removes duplicates and outliers (Z-score), splits sampling period into start/end dates, saves final table |
| DLT | `Project_Code/Project_Code/Code/Delta_Live_Table.ipynb` | Delta Live Tables version of the pipeline |

## Infrastructure

`Project_Code/Project_Code/Terraform/` provisions the Azure resources (storage accounts, ADLS Gen2,
SQL server/database, Data Factory, Logic App, Databricks workspace).
See `Project_Code/Project_Code/Terraform/Instructions.md`.

Create your own `secrets.tfvars` (not committed) with `sql_password`,
`azure_ad_admin_user`, `azure_ad_admin_object_id` and `my_public_ip`.

## Data

The dataset is not included (large files). Download the EEA Waterbase WISE6
aggregated data, upload it to your ADLS container, then run the notebooks in
order: Bronze, Silver, Gold.

## Setup notes

- Replace `<storage_access_key>` in the notebooks with your own key
  (preferably via a Databricks secret scope).
- Make sure `/mnt/datalake` is mounted to your ADLS container.
