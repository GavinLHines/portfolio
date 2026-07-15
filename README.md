# Gavin Hines Portfolio

A portfolio of my technical work. Most of it comes from my Data Science Internship at **CloudXSystems** (summer 2026), and it also includes academic work from my coursework at Flagler College. Internship projects sit in the top-level folders below; college work lives in `college-projects/`.

> [!IMPORTANT]
> ## Internship source code is available on request
>
> The CloudXSystems code is **proprietary**, so this public repository contains **written descriptions rather than source**.
>
> The actual **source code, Git patches with signed commit history, merged pull requests, and run output** are kept in a private repository and shared with prospective employers **on request**.
>
> **Please reach out** and I will share the private repository or walk through any project directly.
>
> **To request access, email [gavinlanier222@gmail.com](mailto:gavinlanier222@gmail.com).**

## Internship projects (CloudXSystems)

### 1. OutboundSuppliesReports — incremental dbt billing mart

A production incremental dbt mart that calculates what customers are billed for outbound packaging supplies on a 3PL warehouse-management platform. It translates the backend CostPlus pricing rules into SQL and snapshots cost and price at shipment time so charges stay auditable even when pricing rules change later.

- **Scale:** 28.4M-row shipment source.
- **Technical:** merge incremental strategy on a hashed surrogate key; append-only schema evolution; ~11 joins across 4 schemas; 27 documented columns with semantic-layer metrics; production indexes, identity columns, and grants managed through dbt pre/post-hooks.
- **Skills:** dbt, SQL, data modeling, PostgreSQL.
- **Proof:** source code, 4-commit Git patch, and merged PR available on request.

### 2. stg_relocation_items — incremental dbt staging model

A production incremental dbt staging model that converts a legacy inventory log into a clean, auditable movement history. Each row covers one relocation event with a full before/after view: asset, location, quantity, user, and timestamp.

- **Scale:** 1,815,913 relocation events (Nov 2020 – Jun 2026), ~22,000/month recently.
- **Technical:** delete+insert incremental strategy on a hashed surrogate key; a 7-day incremental run scans ~0.3% of the table (~99% fewer rows per build than a full rebuild); 10 left joins across 5 schemas; timezone-aware date conversion; unique and not_null tests on the surrogate key.
- **Skills:** dbt, SQL, data modeling, PostgreSQL.
- **Proof:** source code, 5-commit Git patch, and merged PR available on request.

### 3. Schedule A Fee Rate Extraction — n8n automation

A 30-node n8n pipeline that triggers on PDF upload to Google Drive, runs a 6-stage LLM chain to extract structured fee data from contracts, and loads results to PostgreSQL and Google Sheets with automated Gmail notifications. Migrated from Zapier; uses Claude Haiku through native Anthropic nodes.

- **Scale:** 26+ contracts, live in production.
- **Skills:** n8n, JavaScript, Google Drive, PostgreSQL, Claude Haiku, Zapier migration.
- **Proof:** exported workflow and walkthrough available on request.

### 4. Operations Key Alerts — n8n automation

A 10-node n8n workflow that polls Slack for operations alerts, routes them through three parallel paths (inventory booking, fulfillment summary, error extraction), uses Claude Haiku for reformatting and structured extraction, and posts results to Google Chat. Migrated from Zapier with a polling-based trigger design.

- **Scale:** monitors an operations-alert channel, live in production.
- **Skills:** n8n, Slack API, Google Chat API, Claude Haiku, JavaScript.
- **Proof:** exported workflow and walkthrough available on request.

---

## College projects

These are academic projects from my Flagler College coursework. None of it is CloudXSystems work, and none of it contains proprietary code. Full source and data are included in `college-projects/`.

### Mangrove Tree Data Analysis (R)

An exploratory data analysis in R, done as a team of three for the CIS 351 B final project (Spring 2026). We analyzed lab chemistry data from 126 mangrove samples across seven Florida sites, cleaned the raw data, and fixed a unit-conversion error in the original researchers' formulas. We built the visualizations in ggplot2 and ran non-parametric tests, which showed organic matter differs across sites (Kruskal-Wallis, p = 3.03e-13) but not between the two species (Wilcoxon rank-sum).

- **Skills:** R, tidyverse, ggplot2, ggpubr, data cleaning, regex, non-parametric statistics.
- **Contents:** `college-projects/mangrove-data-analysis/` (rendered R report, source CSV, assignment rubric, notes).

---

## Repository structure

```
gavinhines-portfolio/
  README.md
  outbound-supplies-report/
    notes.md
  stg-relocation-items/
    notes.md
  schedule-a-migration/
    notes.md
  ops-key-alerts/
    notes.md
  college-projects/
    README.md
    mangrove-data-analysis/
      Final_Project_Submission.html
      source_data.csv
      assignment_rubric.html
      notes.md
```
