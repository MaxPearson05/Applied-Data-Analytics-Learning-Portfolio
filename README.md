# Applied Data Analytics Learning Portfolio

A practical learning portfolio documenting the development of end-to-end data analytics skills through structured exercises, real public datasets and applied projects.

The work covers data ingestion, cleaning, relational modelling, SQL analysis, Python/pandas, Power Query, Power BI, DAX, validation and stakeholder-focused communication.

This repository is a learning record as well as a project portfolio. It shows how the work was developed, tested and improved rather than presenting every project as a single finished deliverable.

## Learning structure

The repository uses a day-by-day structure based on a guided analytics learning plan. The day numbers describe the intended learning sequence and evidence structure; they do not claim that the work was completed in exactly twelve calendar days.

Alongside the guided project work, I studied analytics theory independently and completed SQL practice through StrataScratch. These exercises strengthened my understanding of SQL logic, joins, aggregation, window functions, data quality and analytical reasoning.

## Core tools

**Excel | Power Query | PostgreSQL | SQL | Python | pandas | BigQuery | Power BI | DAX | Git/GitHub**

Tableau is part of my broader learning stack but is not represented by a completed project in this repository.

## Learning journey

### Stage 1 — Data foundations

Built a financial analysis workflow using Microsoft's public Financial Sample dataset.

Work completed included:

- Excel tables and structured references
- `SUMIFS`, `COUNTIFS`, `AVERAGEIFS` and `XLOOKUP`
- Dynamic arrays and validation controls
- Power Query staging and transformation
- PivotTables, slicers and KPI dashboards
- PostgreSQL table creation and data import
- Aggregation, filtering and grouped analysis
- Weighted profit-margin calculations
- Excel-to-PostgreSQL reconciliation
- SQL retrieval and error repair

### Stage 2 — Relational SQL, Python and healthcare analytics

Developed relational modelling and clinical-trials analysis skills using ClinicalTrials.gov and the AACT relational database.

Work completed included:

- Joins, anti-joins, CTEs and subqueries
- Primary keys, foreign keys and table grain
- Window functions including `LAG`, `ROW_NUMBER`, `RANK` and `DENSE_RANK`
- ClinicalTrials.gov API v2 ingestion
- Nested JSON normalisation with Python and pandas
- A controlled 200-study development extract
- AACT source profiling
- Trial-level fact-table design
- One-to-many bridge tables
- PostgreSQL portfolio analysis
- Power BI data modelling and DAX measures

## Flagship project — Clinical Trial Portfolio & Delivery Intelligence

A real-data healthcare analytics project using ClinicalTrials.gov and the AACT relational database.

The analysis focuses on industry-led interventional drug and biological studies starting from 2015 onward, using a global cohort with a UK lens.

### Business questions

The project investigates:

- How industry-sponsored trial activity changes over time
- How trials are distributed across development phases
- How enrollment, sites and duration change through development
- Which countries participate most heavily in clinical research
- How the UK's participation changes over time
- Which phases show greater recorded discontinuation rates
- Where public results-reporting coverage is weakest

### Data model

The Power BI model uses:

- `FactTrials` — one row per trial, identified by `nct_id`
- `BridgeTrialCountry` — one row per trial-country participation record

The bridge table allows multinational trials to be represented across participating countries without duplicating the trial-level fact table.

### Dashboard pages

#### Trial Overview

Summarises trial volume, status, phase, annual activity and country participation.

#### Portfolio & Delivery Insights

Examines enrollment, site footprint, duration and discontinuation risk by development phase.

#### Geographic & Reporting Governance

Analyses global participation, UK participation over time, results coverage and the leading international markets.

### Overall dashboard results

The final Power BI model displayed:

| Metric | Result |
|---|---:|
| Total trials | 46,955 |
| Completed trials | 24,633 |
| Terminated trials | 4,609 |
| Terminated share | 9.8% |
| UK trials | 6,452 |
| Known geography trials | 44,091 |
| UK participation share | 14.6% |
| Mature eligible trials | 22,021 |
| Posted results trials | 9,513 |
| Results coverage | 43.2% |

Counts are tied to the relevant data extract and refresh stage. Earlier API and AACT warehouse stages contain separately documented snapshot counts, so those values should not be presented as if they were produced by one identical refresh.

### Key findings

- Trial-start activity reached a series high of approximately 4,753 studies in 2021.
- Later-stage trials generally require greater enrollment and wider site footprints.
- UK participation declined from approximately 18% in earlier years to approximately 13% by 2025.
- Results-reporting coverage differs substantially by development phase.
- The United States is the largest country market by trial participation, followed by China and other major international markets.

These findings are descriptive and do not establish causal relationships.

## Analytical principles

### Define grain before analysis

Every analytical table has an explicit row-level grain and identified keys before calculations or joins are performed.

### Protect against row multiplication

One-to-many and many-to-many relationships are handled using pre-aggregation, bridge tables, `EXISTS`, `COUNT(DISTINCT ...)`, key validation and before-and-after reconciliation.

### Validate important numbers

The financial learning project includes Excel, Power Query and PostgreSQL reconciliation. The clinical-trials project includes structural QA, relationship validation and checks against the SQL outputs used to build the Power BI model.

Excel validation should not be inferred for the clinical-trials project because Excel was not used as a validation tool for that project.

### Separate evidence from interpretation

Descriptive relationships are not presented as causal effects without an appropriate causal design.

### Analyse for decisions

Calculations and visuals are designed around stakeholder questions rather than being created simply because the data allows them.

## Data quality and validation

Quality assurance is treated as part of the analysis rather than an afterthought.

Checks included:

- Row-count reconciliation
- Primary-key uniqueness
- Duplicate detection
- Missing-value profiling
- Foreign-key validation
- Join-cardinality checks
- Aggregate reconciliation
- Denominator validation
- Date-range validation
- Filter-slice testing
- Relationship validation in Power BI
- Negative enrollment and duration checks

The clinical-trials model retained missing phase values as `Not reported` rather than silently excluding them.

## SQL practice and theory development

Alongside the applied projects, SQL practice and independent theory study covered:

- `WHERE`, `GROUP BY` and `HAVING`
- `CASE` expressions
- Inner, left, full and anti-joins
- CTEs and subqueries
- Conditional aggregation
- Weighted metrics and null handling
- Date and time analysis
- `LAG`, `LEAD` and ranking functions
- Running totals and rolling calculations
- Table grain and cardinality
- Validation and reconciliation

StrataScratch exercises are retained as learning evidence and demonstrate closed-book retrieval, error diagnosis and query reconstruction.

## Explore the repository

### Stage 1 — Data foundations

Excel, Power Query, PostgreSQL, SQL joins, dimensional thinking and data-quality checks.

[View Stage 1](Stage-01-data-foundations/)

### Stage 2 — Clinical trials analytics

Python ingestion, AACT relational modelling, SQL analysis, Power BI and DAX.

[View Stage 2](Stage-02-relational-sql-python-healthcare/)

### Featured project

[Clinical Trials Intelligence Power BI project](Stage-02-relational-sql-python-healthcare/day-12-clinical-trials-powerbi-model/)

## Public-data and privacy controls

The repository does not contain credentials, passwords, private connection strings or sensitive personal information.

Large raw AACT extracts are not required for the employer-facing project repository. The final project repository should publish the methodology, SQL, DAX documentation, screenshots, PDF export and safe derived outputs rather than a large raw database snapshot.

## Next step

The learning repository records the development process. A separate `clinical-trials-intelligence` repository will present the finished project in a shorter employer-facing format with an executive summary, dashboard screenshots, technical methodology, key findings, limitations and reproducibility guidance.
