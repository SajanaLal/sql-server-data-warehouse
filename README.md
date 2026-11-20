# Layered Data Warehouse: Bronze → Silver → Gold (SQL Server)

A medallion-architecture data warehouse that integrates **6 CRM and ERP source files (~116K records)** into analysis-ready datasets, with automated ingestion, data cleaning and validation at each layer.

> Built as a guided learning project following ["SQL Data Warehouse from Scratch"](https://www.youtube.com/watch?v=9GVqKuTVANE&list=PLNcg_FV9n7qaUWeyUkPfiVtMbKlrfMqA8) by Data with Baraa. The source datasets are course sample data. I worked through the build, ran and tested every layer.

## Architecture

- **Bronze**: raw ingestion of CRM and ERP source files via the `bronze.load_bronze` stored procedure, with BULK INSERT, execution-time logging and TRY/CATCH error handling
- **Silver**: cleaned and standardised tables, loaded by `silver.load_silver`, with duplicates and nulls removed and date, sales and pricing inconsistencies corrected
- **Gold**: star-schema fact and dimension views with surrogate keys and multi-source joins, ready for BI reporting

## Sources

| System | Files |
| --- | --- |
| CRM | cust_info, prd_info, sales_details |
| ERP | CUST_AZ12, LOC_A101, PX_CAT_G1V2 |

## Repository structure

```
scripts/
  init_database.sql        create the DataWarehouse database and the bronze, silver and gold schemas
  bronze/                  ddl_bronze.sql, proc_load_bronze.sql
  silver/                  ddl_silver.sql, proc_load_silver.sql
  gold/                    ddl_gold.sql
tests/
  quality_checks_silver.sql
  quality_checks_gold.sql
```

## How to run

1. Run `scripts/init_database.sql`. **Warning:** it drops and recreates the `DataWarehouse` database if it already exists.
2. Run the bronze and silver DDL scripts, then `scripts/gold/ddl_gold.sql`.
3. In `proc_load_bronze.sql`, change the `C:\path\to\sql-server-data-warehouse\` prefix in each BULK INSERT to where the CSV files are on your machine.
4. Run `EXEC bronze.load_bronze;` then `EXEC silver.load_silver;`.
5. Query the `gold` views and run the scripts in `tests/`.

## Data quality checks

- Key uniqueness on dimensions and facts
- Referential integrity between fact and dimension views
- Date logic validation (order dates, ship dates, due dates)
- Sales calculation verification against source records

## Tools

SQL Server (stored procedures, BULK INSERT, views, CTEs, window functions)

## What it demonstrates

Warehouse layering, automated and logged ETL, dimensional modelling, and treating data quality as a tested requirement rather than an assumption.
