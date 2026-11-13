# Customer Lifecycle & Campaign Effectiveness

## Project Overview

I built a synthetic customer analytics project to compare qualifying activity between treatment and holdout groups across three campaigns. I configured corresponding Contacts, Campaigns, and Campaign Members in Salesforce, analyzed the synthetic customer, assignment, campaign, and event files in Databricks, and presented the results in Tableau.

The project demonstrates a guided CRM-to-analytics workflow. The data is synthetic, so the results illustrate the analysis process; they do not predict how real campaigns would perform.

## Live Dashboard

[View the Customer Lifecycle & Campaign Effectiveness dashboard on Tableau Public](https://public.tableau.com/app/profile/rodolfo.flores4240/viz/CustomerLifecycleCampaignEffectiveness/CustomerLifecycleCampaignEffectiveness)

## Business Question

Within each campaign, how did the 30-day qualifying-activity rate compare between customers assigned to treatment and those assigned to holdout? Which results suggest areas for further testing?

## Data and Tools

The project used synthetic data for 1,200 customers, 1,200 campaign assignments, three campaigns, and 8,500 unique customer activity events after removing one exact duplicate from the raw event file.

- **Salesforce Developer Edition:** I configured the CRM Contacts and Campaigns and imported Campaign Members with Holdout, Treatment Delivered, or Treatment Not Delivered status.
- **Excel:** I combined the assignment data with Salesforce Contact and Campaign ID exports to map records and prepare the Campaign Member import file.
- **Databricks Free Edition:** I loaded the original synthetic CSV files, checked and prepared the data, and used SQL to build analysis tables and summary results.
- **Tableau Public:** I built a dashboard from aggregated Databricks summary CSVs.
- **AI assistance:** I used AI assistance to draft and troubleshoot SQL and documentation, then ran the queries and checked their results.

The original synthetic CSVs were created before the Salesforce setup and loaded separately into Databricks. Salesforce exports were used as ID crosswalks to prepare the Campaign Member import; they were not the source files loaded into Databricks. The customer-event data was modeled separately and had no corresponding Salesforce event records. GCP was not used in this project.

## Methodology

1. The project used synthetic customer, campaign, assignment, and activity-event CSV files.
2. I used the synthetic customer and campaign data to configure corresponding records in Salesforce.
3. I exported Salesforce Contact and Campaign ID crosswalks. In Excel, I matched those IDs to the synthetic customer and campaign IDs, then prepared the Campaign Member import file.
4. I loaded the original synthetic CSVs separately into Databricks. I checked record counts and relationships, removed one exact duplicate event from a new table, standardized relevant fields, and prepared analysis tables.
5. I measured whether each assigned customer had at least one qualifying activity during the 30 days after assignment. The qualifying event types were account login, repeat account use, and self-service action.
6. I compared treatment and holdout rates by campaign and calculated percentage-point lift with approximate Bonferroni-adjusted 95% confidence intervals across the three campaign comparisons.
7. I exported aggregate campaign and lifecycle summaries from Databricks to CSV and used them to build the Tableau dashboard.

The assignment groups were balanced at 200 treatment and 200 holdout customers per campaign. They were generated synthetically; they were not created through a genuine randomized assignment process.

## Findings

Treatment-group activity rates were higher than holdout rates in all three synthetic campaign scenarios:

- **Customer Engagement Check-in:** 29.50% treatment versus 23.50% holdout; observed lift of **6 percentage points**. Its adjusted confidence interval was **−4.54 to +16.54 points**, which includes zero. The result is inconclusive.
- **Welcome and First Activity:** 40.50% treatment versus 27.50% holdout; observed lift of **13 points**. Its adjusted interval was **+1.77 to +24.23 points**, above zero in this simulation.
- **Dormant Customer Re-engagement:** 18.50% treatment versus 9.50% holdout; observed lift of **9 points**. Its adjusted interval was **+0.76 to +17.24 points**, above zero in this simulation, with a lower bound close to zero.

The lifecycle breakdown showed that 385 of 400 onboarding assignments were classified as New, while 307 of 400 winback assignments were classified as Dormant and 93 as Active under the project’s definitions. The Onboarding–Dormant segment had only 15 customers total, so its segment rates are especially uncertain.

## Limitations

- All records and outcomes are synthetic. The campaign results demonstrate an analytical workflow, not real customer behavior or business impact.
- Treatment and holdout assignments were balanced but not genuinely randomized. Within each synthetic campaign cohort, sorted customer IDs were alternated between groups.
- The exact campaign message, offer, or other treatment intervention was not specified.
- Lifecycle stages use project-defined rules and are not universal marketing definitions.
- The Onboarding–Dormant subgroup has only 7 holdout and 8 treatment customers. Segment comparisons are descriptive and were not included in the campaign-level confidence-interval adjustment.
- The confidence intervals are approximate and should be interpreted within the limits of this synthetic design.

## Recommended Next Step

The results point to two questions worth checking with real campaign data:

- **Customer Engagement Check-in:** Treatment was 6 percentage points higher than holdout, but the adjusted interval includes zero. That means this test can’t tell us whether the difference reflects a real improvement. Before deciding to stop or change the campaign, run a real test with a clearly defined message and enough customers to judge the result more confidently.
- **Dormant Customer Re-engagement:** 93 of the 400 assigned customers were classified as Active under our project rules, even though the campaign was intended for dormant customers. Review the audience rule and test whether a dormant-only audience is more appropriate. The synthetic results don’t prove that contacting Active customers wastes resources; their inclusion is a reason to check the targeting.

These are proposed tests, not proven business actions: the project uses synthetic data and cannot establish what would happen with real customers.
