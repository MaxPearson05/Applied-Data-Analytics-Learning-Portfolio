\# DAX Measures — Clinical Trials Intelligence



\## Purpose



This document records the DAX measures used in the Clinical Trials Intelligence Power BI dashboard.



The report contains three pages:



1\. Trial Overview

2\. Portfolio \& Delivery Insights

3\. Geographic \& Reporting Governance



The model uses:



\- `FactTrials` as the trial-level fact table

\- `BridgeTrialCountry` as the country participation bridge table

\- `FactTrials\[nct\_id]` as the unique trial identifier



Measures use `DISTINCTCOUNT` to avoid duplicate trial counting.



\---



\## Page 1 — Trial Overview



\### Total Trials



&#x20;   Total Trials =

&#x20;   DISTINCTCOUNT(FactTrials\[nct\_id])



Counts the total number of unique trials in the curated cohort.



\### Completed Trials



&#x20;   Completed Trials =

&#x20;   CALCULATE(

&#x20;       DISTINCTCOUNT(FactTrials\[nct\_id]),

&#x20;       FactTrials\[overall\_status] = "COMPLETED"

&#x20;   )



Counts unique completed trials.



\### Terminated Trials



&#x20;   Terminated Trials =

&#x20;   CALCULATE(

&#x20;       DISTINCTCOUNT(FactTrials\[nct\_id]),

&#x20;       FactTrials\[overall\_status] = "TERMINATED"

&#x20;   )



Counts unique terminated trials.



\### Terminated Share



&#x20;   Terminated Share =

&#x20;   DIVIDE(

&#x20;       \[Terminated Trials],

&#x20;       \[Total Trials]

&#x20;   )



Calculates terminated trials as a proportion of all trials.



\### Trials by Country



&#x20;   Trials by Country =

&#x20;   DISTINCTCOUNT(BridgeTrialCountry\[nct\_id])



Counts unique trials participating in each country.



Because multinational trials can participate in multiple countries, country totals represent participation counts rather than mutually exclusive trial totals.



\---



\## Page 2 — Portfolio \& Delivery Insights



\### Median Enrollment



&#x20;   Median Enrollment =

&#x20;   MEDIAN(FactTrials\[enrollment])



Calculates median planned trial enrollment.



\### 75th Percentile Enrollment



&#x20;   75th Percentile Enrollment =

&#x20;   PERCENTILEX.INC(

&#x20;       FactTrials,

&#x20;       FactTrials\[enrollment],

&#x20;       0.75

&#x20;   )



Calculates the 75th percentile of planned trial enrollment.



\### Median Sites



&#x20;   Median Sites =

&#x20;   MEDIAN(FactTrials\[site\_count])



Calculates the median number of trial sites.



\### Median Countries



&#x20;   Median Countries =

&#x20;   MEDIAN(FactTrials\[country\_count])



Calculates the median number of participating countries per trial.



\### Median Duration Months



&#x20;   Median Duration Months =

&#x20;   DIVIDE(

&#x20;       MEDIAN(FactTrials\[duration\_days]),

&#x20;       30.4375

&#x20;   )



Converts median trial duration from days into months.



\### Final Outcome Trials



&#x20;   Final Outcome Trials =

&#x20;   CALCULATE(

&#x20;       DISTINCTCOUNT(FactTrials\[nct\_id]),

&#x20;       FactTrials\[overall\_status]

&#x20;           IN {

&#x20;               "COMPLETED",

&#x20;               "TERMINATED",

&#x20;               "WITHDRAWN"

&#x20;           }

&#x20;   )



Counts trials with a final recorded outcome status.



\### Discontinuation Rate



&#x20;   Discontinuation Rate =

&#x20;   DIVIDE(

&#x20;       \[Terminated Trials],

&#x20;       \[Final Outcome Trials]

&#x20;   )



Calculates terminated trials as a proportion of trials with a final outcome status.



\---



\## Page 3 — Geographic \& Reporting Governance



\### UK Trials



&#x20;   UK Trials =

&#x20;   CALCULATE(

&#x20;       DISTINCTCOUNT(FactTrials\[nct\_id]),

&#x20;       FactTrials\[uk\_participation] = TRUE()

&#x20;   )



Counts unique trials with recorded UK participation.



\### Known Geography Trials



&#x20;   Known Geography Trials =

&#x20;   CALCULATE(

&#x20;       DISTINCTCOUNT(FactTrials\[nct\_id]),

&#x20;       FactTrials\[country\_count] > 0

&#x20;   )



Counts trials with at least one recorded participating country.



\### UK Participation Share



&#x20;   UK Participation Share =

&#x20;   DIVIDE(

&#x20;       \[UK Trials],

&#x20;       \[Known Geography Trials]

&#x20;   )



Calculates the UK participation share among trials with known geography.



\### Mature Eligible Trials



&#x20;   Mature Eligible Trials =

&#x20;   CALCULATE(

&#x20;       DISTINCTCOUNT(FactTrials\[nct\_id]),

&#x20;       FactTrials\[mature\_results\_eligible] = TRUE()

&#x20;   )



Counts trials eligible for results-posting analysis based on the maturity-window logic.



\### Posted Results Trials



&#x20;   Posted Results Trials =

&#x20;   CALCULATE(

&#x20;       DISTINCTCOUNT(FactTrials\[nct\_id]),

&#x20;       FactTrials\[mature\_results\_eligible] = TRUE(),

&#x20;       FactTrials\[results\_posted] = TRUE()

&#x20;   )



Counts mature eligible trials with recorded posted results.



\### Results Coverage



&#x20;   Results Coverage =

&#x20;   DIVIDE(

&#x20;       \[Posted Results Trials],

&#x20;       \[Mature Eligible Trials]

&#x20;   )



Calculates the proportion of mature eligible trials with posted results.



\### Not Posted Results



&#x20;   Not Posted Results =

&#x20;   \[Mature Eligible Trials] - \[Posted Results Trials]



Calculates mature eligible trials without recorded posted results.



This measure is used with `Posted Results Trials` in the 100% stacked results-coverage chart.



\---



\## Display Measures Used During Formatting



These measures were used temporarily to control KPI card rounding.



\### UK Trials Display



&#x20;   UK Trials Display =

&#x20;   FORMAT(

&#x20;       \[UK Trials],

&#x20;       "#,##0"

&#x20;   )



\### Mature Eligible Display



&#x20;   Mature Eligible Display =

&#x20;   FORMAT(

&#x20;       \[Mature Eligible Trials],

&#x20;       "#,##0"

&#x20;   )



The final cards use the original numeric measures, with display units set to `None` and decimal places configured in the visual formatting pane.



\---



\## Overall Results



| Measure | Result |

|---|---:|

| Total Trials | 46,955 |

| Completed Trials | 24,633 |

| Terminated Trials | 4,609 |

| Terminated Share | 9.8% |

| UK Trials | 6,452 |

| Known Geography Trials | 44,091 |

| UK Participation Share | 14.6% |

| Mature Eligible Trials | 22,021 |

| Posted Results Trials | 9,513 |

| Results Coverage | 43.2% |



\---



\## Interpretation Notes



\- Trial-level measures use `FactTrials\[nct\_id]` to avoid duplicate counting.

\- Country measures use `BridgeTrialCountry`, so multinational trials can appear in multiple country totals.

\- Median measures are non-additive. Matrix totals represent the overall median, not the sum or average of phase values.

\- The discontinuation-rate total is recalculated using the overall numerator and denominator.

\- The 100% stacked results chart shows the share of eligible trials, not raw trial counts.

\- Percentages are formatted to one decimal place.

\- Enrollment, site and country counts are formatted as whole numbers.

\- Duration is formatted to one decimal place in months.

