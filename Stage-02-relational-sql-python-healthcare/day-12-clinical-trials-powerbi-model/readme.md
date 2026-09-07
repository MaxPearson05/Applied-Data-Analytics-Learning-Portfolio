# Day 12 — Clinical Trials Intelligence



## Project overview



Built an employer-facing Power BI dashboard using ClinicalTrials.gov AACT data.



The project analyses industry-led interventional clinical trials starting from 2015 onward, with a focus on:



* Trial activity and status
* Development phase mix
* Enrollment and operational burden
* Trial duration and site footprint
* Geographic participation
* UK participation over time
* Discontinuation risk
* Public results-reporting coverage



The project combines SQL data preparation, relational modelling, DAX measures and interactive Power BI visualisation.



\---



## Tools used



* PostgreSQL
* SQL
* Power BI Desktop
* DAX
* Power Query
* Microsoft Bing map visual
* GitHub



\---



## Data scope



The curated cohort includes:



* Interventional studies
* Industry-led trials
* Trials starting from 2015 onward
* Trials involving a drug or biological intervention
* Trials with recorded conditions and interventions



The model uses one row per trial in `FactTrials`.



A separate `BridgeTrialCountry` table stores trial-country participation so multinational trials can be analysed geographically without duplicating the trial-level fact table.



\---



## Dashboard structure



### Page 1 — Trial Overview



Provides an executive summary of the clinical-trial portfolio.



Visuals include:



* Total trials
* Completed trials
* Terminated trials
* Terminated share
* Trials by status
* Trials by development phase
* Trial starts by year
* Top countries by trial participation



### Page 2 — Portfolio \& Delivery Insights



Analyses the operational burden and delivery profile of trials by development phase.



Visuals include:



* 75th percentile enrollment
* Median enrollment
* Median sites
* Median duration
* Operational burden by development phase
* Phase profile scatter chart
* Discontinuation risk matrix
* Conditional formatting to highlight higher-risk phases



### Page 3 — Geographic \& Reporting Governance



Examines global participation and public reporting visibility.



Visuals include:



* UK trial count
* UK participation share
* Mature eligible trial count
* Results coverage
* Global trial participation map
* UK participation share over time
* Results reporting coverage by phase
* Top eight markets by trial participation



\---



## Key results



The dashboard returned the following overall results:



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



Additional findings from the analysis include:



* Trial activity peaked in 2021 at approximately 4,753 trial starts.
* Trial activity decreased after the 2021 peak but remained substantial through 2025.
* Larger development phases generally require greater enrollment and site footprints.
* UK participation declined from approximately 18% in earlier years to approximately 13% by 2025.
* Results coverage varies substantially by development phase.
* The United States is the largest market by trial participation, followed by China and other major international markets.
* Multinational trials contribute to multiple country participation totals.



\---



## SQL work completed



The SQL analysis investigated:



1\. Annual trial activity

2\. Development phase mix

3\. Sponsor concentration

4\. Enrollment burden by phase

5\. Site and geographic footprint

6\. Completed-trial duration

7\. UK participation trends

8\. Country-level market shifts

9\. Discontinuation risk

10\. Raw trial stop reasons

11\. Categorised stop reasons

12\. Results-posting coverage



The SQL outputs were used to validate the Power BI model and support the final dashboard findings.



\---



## Power BI modelling



The Power BI model contains:



* `FactTrials` — trial-level fact table
* `BridgeTrialCountry` — trial-country bridge table



The relationship is:



```text

BridgeTrialCountry\[nct\_id]

           ↕

FactTrials\[nct\_id]

```



This allows trial-level measures and country participation measures to be analysed together while preserving the correct grain of the data.



\---



## DAX work completed



Created DAX measures for:



* Trial counts
* Completed and terminated trials
* Terminated share
* Median enrollment
* 75th percentile enrollment
* Median sites
* Median countries
* Median duration
* Final outcome trials
* Discontinuation rate
* UK trial participation
* UK participation share
* Mature eligible trials
* Posted results trials
* Results coverage
* Not-posted results



The complete measure documentation is available in:



```text

dax/dax\_measures.md

```



\---



## Data-quality and validation checks



The project included checks for:



* Duplicate trial identifiers
* Missing trial identifiers
* Missing phase values
* Negative enrollment values
* Negative duration values
* Correct relationship cardinality
* Trial-count reconciliation between SQL and Power BI
* Correct percentage formatting
* Correct country participation logic
* Correct filtering of future or incomplete years from the historical trend



The model retained missing phase values as `Not reported` rather than silently excluding them.



\---



## What I learned



* How to design a trial-level fact table.
* Why a bridge table is required for multinational trial participation.
* How to use `DISTINCTCOUNT` to prevent duplicate trial counting.
* How to create DAX measures for ratios, medians and percentiles.
* Why median totals are non-additive.
* How to build interactive cross-filtering in Power BI.
* How to use conditional formatting to communicate risk.
* How to combine maps, scatter charts, matrix tables and time-series visuals.
* How to validate dashboard outputs against SQL results.
* How to document assumptions and limitations for an employer-facing project.



\---



## Limitations



* Country totals represent trial participation and are not mutually exclusive trial counts.
* Results coverage depends on the definition of mature eligibility and the availability of posted results.
* Registry status is a recorded administrative status and does not represent clinical success or failure.
* Historical trends exclude incomplete or future years where appropriate.
* The dashboard reflects the available AACT extract and may not represent real-time registry changes.



\## Dashboard screenshots



\### Trial Overview



![Trial Overview](screenshots/Overview.png)



\### Portfolio and Delivery Insights



![Portfolio and Delivery Insights](screenshots/Portfolio%20and%20Delivery%20Insights.png)



\### Geographic and Reporting Governance



![Geographic and Reporting Governance](screenshots/Geographic%20%26%20Reporting%20Governance.png)

## How to view the project

- Open `powerbi/clinical_trials_intelligence.pbix` in Power BI Desktop to interact with the report.
- Use the screenshots above for a quick visual overview.
- Review `dax/dax_measures.md` for the documented measures and business logic.
- Review the Day 11 SQL outputs for the curated data preparation and portfolio analysis.

