# Layered Data Warehouse — Bronze → Silver → Gold (SQL Server)

A medallion-architecture data warehouse that integrates **6 CRM and ERP source files (~116K records)** into analysis-ready datasets, with automated ingestion, data cleaning and validation at each layer.

## Architecture

- **Bronze**: raw ingestion of CRM and ERP source files via stored procedures with BULK INSERT, execution-time logging and TRY/CATCH error handling
- **Silver**: cleaned and standardised tables — duplicates and nulls removed; date, sales and pricing inconsistencies corrected
- **Gold**: star-schema fact and dimension views with surrogate keys and multi-source joins, ready for BI reporting

## Data quality checks

- Key uniqueness on dimensions and facts
- Referential integrity between fact and dimension views
- Date logic validation (order dates, ship dates, due dates)
- Sales calculation verification against source records

## Tools

SQL Server (stored procedures, BULK INSERT, views, CTEs, window functions)

## What it demonstrates

Warehouse layering, automated and logged ETL, dimensional modelling, and treating data quality as a tested requirement rather than an assumption.
