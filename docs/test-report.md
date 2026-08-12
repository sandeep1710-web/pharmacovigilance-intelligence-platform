# Test Report — Success Criteria Validation

This report consolidates the validation checks run across each stage of the pipeline, confirming the platform meets its defined success criteria.

## 1. Ingestion (RAW Layer)

| Check | Result |
|---|---|
| Raw table row count matches source file | ✅ 83 rows, 19 columns |
| Data quality issues documented | ✅ 6 distinct issues identified (see DQ Rulebook) |

## 2. Cleansing & Standardization (STAGING Layer)

| Check | Query | Result |
|---|---|---|
| Zero duplicate REPORT_IDs in staging | `SELECT REPORT_ID, COUNT(*) FROM STAGING.AE_REPORTS_FINAL_STAGING GROUP BY REPORT_ID HAVING COUNT(*) > 1` | ✅ 0 rows returned |
| All dates valid as DATE type | Parse date step applied to ONSET_DATE, REPORT_RECEIVED_DATE across 4 known source formats | ✅ Passed |
| Row count after dedup | — | 81 rows (from 83 raw) |

## 3. Dimensional Modeling (CURATED Layer)

| Check | Result |
|---|---|
| Fact table grain documented | ✅ One row = one AE report |
| Total rows vs distinct REPORT_IDs | ✅ 81 total rows = 81 distinct reports |
| Duplicate REPORT_IDs in fact table | ✅ 0 rows returned |
| Referential integrity — DIM_DRUG | ✅ 0 missing drug keys |
| Referential integrity — DIM_PATIENT | ✅ 0 missing patient keys |
| Referential integrity — DIM_COUNTRY | ✅ 0 missing country keys |
| Referential integrity — DIM_MANUFACTURING | ✅ 0 missing manufacturing keys |
| Referential integrity — Report Received Date | ✅ 0 invalid date keys |
| Referential integrity — Onset Date | ✅ 0 invalid date keys |
| Analytical views return correct results | ✅ VW_AE_COUNTS_BY_DRUG_MONTH, VW_SERIOUS_EVENT_RATE_BY_COUNTRY validated |

## 4. Snowflake ML — Signal Detection

| Check | Result |
|---|---|
| Training dataset window | Jan 2025 – Sep 2025 (41 monthly observations) |
| Prediction dataset window | Oct 2025 – Dec 2025 (16 monthly observations) |
| ML function executed end-to-end | ✅ `SNOWFLAKE.ML.ANOMALY_DETECTION` ran successfully |
| Prediction interval | 0.95 (95%) |
| Genuine anomaly identified | ✅ ONCOZEL flagged (monthly); ONCOZEL + VASCULIN flagged (weekly) |
| Statistical signal vs. clinical causality distinction documented | ✅ Documented — model output does not establish causality |

## 5. Dashboard & Reporting

| Metric | Value |
|---|---|
| Total AE Reports | 81 |
| Total Drugs | 9 |
| Serious Events | 40 |
| Total Anomalies (monthly view) | 1 |
| Total Anomalies (weekly view) | 2 |

## Summary

All defined success criteria across ingestion, cleansing, modeling, ML, and reporting stages were met, with referential integrity checks returning zero orphan records and zero duplicate report IDs at every downstream stage.
