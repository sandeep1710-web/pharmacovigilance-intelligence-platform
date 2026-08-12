# Data Quality (DQ) Rulebook

This document lists all data quality rules applied during the ETL process (primarily in Dataiku DSS) before data was promoted to the Snowflake `STAGING` schema.

## Rule Summary

| Rule ID | Quality Rule | Action Taken |
|---|---|---|
| DQ-001 | Duplicate REPORT_ID | Remove duplicates |
| DQ-002 | REPORT_RECEIVED_DATE must be valid | Convert to DATE |
| DQ-003 | ONSET_DATE must be valid | Convert to DATE |
| DQ-004 | DRUG_NAME must be standardized | Standardize casing |
| DQ-005 | COUNTRY must be standardized | Standardize casing |
| DQ-006 | PATIENT_SEX values standardized | Added 'U' (imputation) |
| DQ-007 | Missing PATIENT_AGE | Retain median (imputation) |
| DQ-008 | Missing DOSE_AMOUNT_MG | Retain NULL (no imputation) |
| DQ-009 | REPORT_ID must be unique | Quality checkpoint |
| DQ-010 | Data loaded to STAGING only after validation | Workflow checkpoint |

---

## Rule Descriptions

### DQ-001 — Duplicate REPORT_ID
Each adverse event report must have a unique `REPORT_ID`. Duplicate records were identified using the `REPORT_ID` field and removed during the Dataiku DSS data preparation process. The first valid occurrence of each report was retained while duplicate entries were discarded, ensuring every adverse event report is represented only once in the staging dataset.

### DQ-002 — REPORT_RECEIVED_DATE Validation
The `REPORT_RECEIVED_DATE` field must contain valid calendar dates in `YYYY-MM-DD` format. All values were validated and converted to the Snowflake `DATE` data type, ensuring consistency for time-based analysis.

### DQ-003 — ONSET_DATE Validation
The `ONSET_DATE` field represents the date the adverse event occurred. It was validated and converted to the standard `DATE` format, ensuring consistent storage and compatibility with analytical queries and ML models.

### DQ-004 — DRUG_NAME Standardization
Drug names contained inconsistent casing due to data-entry variation. All values were standardized to eliminate duplicate representations of the same drug, improving grouping, aggregation, and reporting accuracy.

### DQ-005 — COUNTRY Standardization
Country names were standardized to ensure consistent capitalization throughout the dataset, improving the accuracy of country-level reporting.

### DQ-006 — PATIENT_SEX Standardization
Missing or undefined values in `PATIENT_SEX` were replaced with `'U'` (Unknown). This ensures every record has a valid categorical value while preserving the fact that the original information was unavailable.

### DQ-007 — Missing PATIENT_AGE
Missing values in `PATIENT_AGE` were handled using median imputation, minimizing the impact of missing data while preserving the overall age distribution.

### DQ-008 — Missing DOSE_AMOUNT_MG
Missing values in `DOSE_AMOUNT_MG` were intentionally retained as `NULL`. Since dosage is clinical information, no imputation was performed to avoid introducing misleading values — preserving the integrity of the original clinical data.

### DQ-009 — REPORT_ID Quality Checkpoint
A Dataiku Data Quality Checkpoint verified that all `REPORT_ID` values were unique before export to Snowflake. If duplicates were detected, the workflow would fail validation and block progression.

### DQ-010 — Workflow Validation Checkpoint
A final checkpoint ensured that only datasets satisfying all predefined DQ rules were loaded into the Snowflake `STAGING` schema — guaranteeing downstream modeling, ML, and reporting operate on validated, reliable data.

---

## Initial Data Quality Assessment (Pre-Cleansing)

| Issue | Description | Impact |
|---|---|---|
| Duplicate Records | Duplicate REPORT_ID values | Duplicate reporting |
| Missing Patient Age | Null values | Incomplete demographics |
| Missing Dose | Null dose values | Reduced dosage analysis quality |
| Mixed Text Case | Drug/Country/Sex | Inconsistent grouping |
| Date Format | Multiple date representations | Date parsing issues |
| Missing Optional Fields | Concomitant medication | Incomplete clinical context |
