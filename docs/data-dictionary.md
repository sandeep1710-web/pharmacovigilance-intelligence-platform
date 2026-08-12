# Data Dictionary — CURATED Schema

This document describes the business tables and columns implemented in the Snowflake `CURATED` schema. It serves as a reference for developers, analysts, and business users on the meaning, data type, and purpose of each attribute in the dimensional model.

---

## FACT_AE_REPORT

**Table Description:** Stores one record for each adverse event report.

**Table Grain:** One row = One Adverse Event Report

| Column | Data Type | Description |
|---|---|---|
| REPORT_ID | VARCHAR | Unique adverse event report identifier |
| REPORT_RECEIVED_DATE_KEY | NUMBER | Foreign key referencing DIM_DATE |
| ONSET_DATE_KEY | NUMBER | Foreign key referencing DIM_DATE |
| DRUG_KEY | NUMBER | Foreign key referencing DIM_DRUG |
| PATIENT_KEY | NUMBER | Foreign key referencing DIM_PATIENT |
| COUNTRY_KEY | NUMBER | Foreign key referencing DIM_COUNTRY |
| MANUFACTURING_KEY | NUMBER | Foreign key referencing DIM_MANUFACTURING |
| ADVERSE_EVENT_TERM | VARCHAR | Reported adverse event |
| EVENT_SERIOUSNESS | VARCHAR | Serious / Non-Serious classification |
| OUTCOME | VARCHAR | Outcome of the adverse event |
| DOSE_AMOUNT_MG | FLOAT | Drug dosage administered |
| ROUTE_OF_ADMINISTRATION | VARCHAR | Route of drug administration |
| CONCOMITANT_MEDICATIONS | VARCHAR | Additional medications taken |
| CAUSALITY_ASSESSMENT | VARCHAR | Initial causality assessment |

---

## DIM_DRUG

| Column | Data Type | Description |
|---|---|---|
| DRUG_KEY | NUMBER | Surrogate key |
| DRUG_NAME | VARCHAR | Name of drug |
| DRUG_NDC_CODE | VARCHAR | National Drug Code |
| INDICATION | VARCHAR | Therapeutic indication |

---

## DIM_PATIENT

| Column | Description |
|---|---|
| PATIENT_KEY | Surrogate key |
| PATIENT_AGE | Patient age |
| PATIENT_SEX | Gender |
| REPORTER_TYPE | Healthcare professional / Consumer |

---

## DIM_COUNTRY

| Column | Description |
|---|---|
| COUNTRY_KEY | Surrogate key |
| COUNTRY | Reporting country |

---

## DIM_MANUFACTURING

| Column | Description |
|---|---|
| MANUFACTURING_KEY | Surrogate key |
| BATCH_ID | Manufacturing batch |
| MANUFACTURING_SITE | Manufacturing site |

---

## DIM_DATE

| Column | Description |
|---|---|
| DATE_KEY | Surrogate key |
| FULL_DATE | Calendar date |
| DAY | Day of month |
| MONTH | Month number |
| MONTH_NAME | Month name |
| QUARTER | Quarter |
| YEAR | Calendar year |

---

## Analytical Views

| View | Purpose |
|---|---|
| VW_AE_COUNTS_BY_DRUG_MONTH | Monthly adverse event counts by drug |
| VW_SERIOUS_EVENT_RATE_BY_COUNTRY | Serious adverse event rate by country |

---

## ML Output Table — AE_SIGNAL_FLAGS

| Column | Description |
|---|---|
| SERIES | Drug name |
| TS | Reporting month |
| Y | Actual adverse event count |
| FORECAST | Expected adverse event count |
| LOWER_BOUND | Lower prediction interval |
| UPPER_BOUND | Upper prediction interval |
| IS_ANOMALY | Statistical anomaly flag |
