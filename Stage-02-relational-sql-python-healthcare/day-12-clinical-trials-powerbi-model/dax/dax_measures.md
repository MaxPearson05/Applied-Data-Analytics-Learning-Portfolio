# DAX Measures — Clinical Trials Intelligence



## Purpose



This document records the DAX measures used in the Clinical Trials Intelligence Power BI dashboard.



The report contains three pages:



1. Trial Overview

2. Portfolio & Delivery Insights

3. Geographic & Reporting Governance



The model uses:



- `FactTrials` as the trial-level fact table

- `BridgeTrialCountry` as the country participation bridge table

- `FactTrials[nct_id]` as the unique trial identifier



Measures use `DISTINCTCOUNT` to avoid duplicate trial counting.



---



## Page 1 — Trial Overview



### Total Trials



       Total Trials =

       DISTINCTCOUNT(FactTrials[nct_id])



Counts the total number of unique trials in the curated cohort.



### Completed Trials



       Completed Trials =

       CALCULATE(

           DISTINCTCOUNT(FactTrials[nct_id]),

           FactTrials[overall_status] = "COMPLETED"

       )



Counts unique completed trials.



### Terminated Trials



       Terminated Trials =

       CALCULATE(

           DISTINCTCOUNT(FactTrials[nct_id]),

           FactTrials[overall_status] = "TERMINATED"

       )



Counts unique terminated trials.



### Terminated Share



       Terminated Share =

       DIVIDE(

           [Terminated Trials],

           [Total Trials]

       )



Calculates terminated trials as a proportion of all trials.



### Trials by Country



       Trials by Country =

       DISTINCTCOUNT(BridgeTrialCountry[nct_id])



Counts unique trials participating in each country.



Because multinational trials can participate in multiple countries, country totals represent participation counts rather than mutually exclusive trial totals.



---



## Page 2 — Portfolio & Delivery Insights



### Median Enrollment



       Median Enrollment =

       MEDIAN(FactTrials[enrollment])



Calculates median planned trial enrollment.



### 75th Percentile Enrollment



       75th Percentile Enrollment =

       PERCENTILEX.INC(

           FactTrials,

           FactTrials[enrollment],

           0.75

       )



Calculates the 75th percentile of planned trial enrollment.



### Median Sites



       Median Sites =

       MEDIAN(FactTrials[site_count])



Calculates the median number of trial sites.



### Median Countries



       Median Countries =

       MEDIAN(FactTrials[country_count])



Calculates the median number of participating countries per trial.



### Median Duration Months



       Median Duration Months =

       DIVIDE(

           MEDIAN(FactTrials[duration_days]),

           30.4375

       )



Converts median trial duration from days into months.



### Final Outcome Trials



       Final Outcome Trials =

       CALCULATE(

           DISTINCTCOUNT(FactTrials[nct_id]),

           FactTrials[overall_status]

               IN {

                   "COMPLETED",

                   "TERMINATED",

                   "WITHDRAWN"

               }

       )



Counts trials with a final recorded outcome status.



### Discontinuation Rate



       Discontinuation Rate =

       DIVIDE(

           [Terminated Trials],

           [Final Outcome Trials]

       )



Calculates terminated trials as a proportion of trials with a final outcome status.



---



## Page 3 — Geographic & Reporting Governance



### UK Trials



       UK Trials =

       CALCULATE(

           DISTINCTCOUNT(FactTrials[nct_id]),

           FactTrials[uk_participation] = TRUE()

       )



Counts unique trials with recorded UK participation.



### Known Geography Trials



       Known Geography Trials =

       CALCULATE(

           DISTINCTCOUNT(FactTrials[nct_id]),

           FactTrials[country_count] > 0

       )



Counts trials with at least one recorded participating country.



### UK Participation Share



       UK Participation Share =

       DIVIDE(

           [UK Trials],

           [Known Geography Trials]

       )



Calculates the UK participation share among trials with known geography.



### Mature Eligible Trials



       Mature Eligible Trials =

       CALCULATE(

           DISTINCTCOUNT(FactTrials[nct_id]),

           FactTrials[mature_results_eligible] = TRUE()

       )



Counts trials eligible for results-posting analysis based on the maturity-window logic.



### Posted Results Trials



       Posted Results Trials =

       CALCULATE(

           DISTINCTCOUNT(FactTrials[nct_id]),

           FactTrials[mature_results_eligible] = TRUE(),

           FactTrials[results_posted] = TRUE()

       )



Counts mature eligible trials with recorded posted results.



### Results Coverage



       Results Coverage =

       DIVIDE(

           [Posted Results Trials],

           [Mature Eligible Trials]

       )



Calculates the proportion of mature eligible trials with posted results.



### Not Posted Results



       Not Posted Results =

       [Mature Eligible Trials] - [Posted Results Trials]



Calculates mature eligible trials without recorded posted results.



This measure is used with `Posted Results Trials` in the 100% stacked results-coverage chart.



---



## Display Measures Used During Formatting



These measures were used temporarily to control KPI card rounding.



### UK Trials Display



       UK Trials Display =

       FORMAT(

           [UK Trials],

           "#,##0"

       )



### Mature Eligible Display



       Mature Eligible Display =

       FORMAT(

           [Mature Eligible Trials],

           "#,##0"

       )



The final cards use the original numeric measures, with display units set to `None` and decimal places configured in the visual formatting pane.



---



## Overall Results



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



---



## Interpretation Notes



- Trial-level measures use `FactTrials[nct_id]` to avoid duplicate counting.

- Country measures use `BridgeTrialCountry`, so multinational trials can appear in multiple country totals.

- Median measures are non-additive. Matrix totals represent the overall median, not the sum or average of phase values.

- The discontinuation-rate total is recalculated using the overall numerator and denominator.

- The 100% stacked results chart shows the share of eligible trials, not raw trial counts.

- Percentages are formatted to one decimal place.

- Enrollment, site and country counts are formatted as whole numbers.

- Duration is formatted to one decimal place in months.

