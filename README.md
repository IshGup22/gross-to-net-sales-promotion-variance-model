# Gross-to-Net Sales & Promotion Variance Model

**Portfolio learning project | Excel, Power Query-ready CSVs, and Power BI-ready tables**

## What is completed
- A working formula-driven Excel workbook with Dashboard, Assumptions, Scenarios, Monthly_Model and Read_Me sheets.
- 3,600 reproducible synthetic transactions, aggregated into 48 month-by-category groups.
- Gross sales → promotional discounts → returns/allowances → trade-spend assumptions → net sales → COGS → variable selling → contribution profit.
- Three editable promotional decisions: Protect margin, Maintain baseline, Growth campaign.
- Modeled gross-sales plan variance, formula-linked KPI dashboard and scenario comparison.
- Flat CSV tables to import into Power BI Desktop.

## Crucial data provenance
**The supplied transactional data is SIMULATED, not an extract from UCI or a real company.**
The actual public dataset suggested for a later iteration is:
Chen, D. (2015), Online Retail, UCI Machine Learning Repository. https://doi.org/10.24432/C5BW33
https://archive.ics.uci.edu/dataset/352/online+retail

UCI's data includes quantities, prices, invoice dates and cancellations. It DOES NOT include actual promotion discounts, trade terms, approved forecasts, unit costs, or variable-selling expenses.
Those must be explicit *hypotheses*, not portrayed as measured commercial results.

## Work through the Excel model
1. Open `Gross_to_Net_Promotion_Model.xlsx`.
2. Start at the Dashboard. The dashboard reports the scenario selected in `Assumptions!B8`.
3. Choose **1** (Protect margin), **2** (Maintain baseline), or **3** (Growth campaign).
4. Edit return allowance, trade spend, variable selling and plan growth in `Assumptions!B4:B7`.
5. To change scenario elasticities, edit `Scenarios!B5:C7` (incremental discount / assumed volume change).
6. Review the formulas in `Monthly_Model`. A modeled plan is not a historical approved budget.

### Model definitions
Gross sales = Σ(quantity × selling price)
Observed discounts = synthetic discount estimate (for demo data only)
Scenario gross = gross sales × (1 + assumed volume response)
Scenario discounts = observed discounts × (1 + volume response) + scenario gross × incremental discount
Return/allowances = scenario gross × assumed returns percentage
Trade spend = scenario gross × assumed trade-spend percentage
Net sales = scenario gross − discounts − returns − trade spend
COGS = assumed baseline COGS × (1 + assumed volume response)
Variable selling = scenario gross × assumed selling percentage
Contribution profit = net sales − COGS − variable selling
Contribution margin = contribution profit / net sales

**Important nuance:** “Contribution profit” is not audited company net profit. No fixed expenses, taxes or financing costs are modeled. There is no real SAP/TPM data.

## Demo findings (from 3,600 synthetic lines; £)
- Baseline scenario: gross £527,909; net sales £443,718; contribution profit £148,903; margin 33.56%.
- Protect-margin scenario: contribution profit £163,093; margin 35.85%.
- Growth campaign: contribution profit £149,034; margin 31.10%.
- Under these assumptions, protect-margin improves **contribution margin by ~2.29 percentage points** vs baseline.
- Under these assumptions, the margin-protection scenario also increases modeled contribution profit by £14,190. These are *model results*, not empirical discoveries.

## Power BI Desktop walkthrough
1. Home → Get data → Text/CSV → import `powerbi_monthly_model.csv`.
2. Import `powerbi_scenario_summary.csv` as a separate table.
3. Set currency fields to Decimal Number; ContributionMargin to Percentage.
4. Create three cards: GrossSales_GBP, NetSales_GBP, Contribution_GBP (from scenario summary).
5. Create a clustered bar chart: Scenario (axis), Contribution_GBP (value).
6. Create a second clustered bar chart: Scenario (axis), ContributionMargin (value).
7. Create a waterfall using scenario finance components, or use a matrix with Gross Sales, Discounts, Returns, Trade Spend and Net Sales.
8. Include a visible note: “Synthetic demo data with explicit spending, returns and cost assumptions.”
9. Save your report as `Gross_to_Net_Promotion_Analysis.pbix` (Power BI Desktop is required; the .pbix is **not** included here).

### Suggested DAX measures for monthly table
```DAX
Gross Sales = SUM(powerbi_monthly_model[GrossSales_GBP])
Modeled Plan = SUM(powerbi_monthly_model[PlanGross_GBP])
Net Sales = SUM(powerbi_monthly_model[BaseNetSales_GBP])
Contribution Profit = SUM(powerbi_monthly_model[BaseContribution_GBP])
Gross Variance = [Gross Sales] - [Modeled Plan]
Gross Variance % = DIVIDE([Gross Variance], [Modeled Plan])
```

## How to switch to genuine public transaction data
1. Download UCI `Online Retail.xlsx` from the link above.
2. In Excel: Data → Get Data → From File → From Workbook; select the UCI file.
3. In Power Query, filter null/zero prices and invalid quantities; **separate cancellations instead of counting them as ordinary sales**.
4. Group by month and product category, or define a documented product-code category mapping.
5. Replace the 48 input aggregates in `Monthly_Model!A4:F51`. If using a different number of groups, extend the model formulas and totals and update the dashboard links.
6. Keep discount %, COGS, trade terms and forecast figures separate as **assumptions**, never attributes purportedly observed in UCI.
7. Validate reconciliation: gross sales − modeled deductions = net sales; net sales − COGS − variable selling = contribution profit.
8. Export the refreshed monthly model and connect Power BI.

## Resume honesty
Use the project as a **synthetic-data, assumption-based personal model** now. Do not say it analyzed actual retail promotional spend. Do not claim SAP or TPM hands-on experience from this project. Do not use previously suggested $1.2M and 2.4 pp figures; they were illustrative and do not match this model.

**Suggested resume wording after reviewing the workbook:**
- Built a formula-driven gross-to-net sales model on 3,600 synthetic retail transactions, separating discounts, allowances and trade-spend assumptions to estimate net sales and contribution margin.
- Compared three promotional spending and volume scenarios using Excel and Power BI-ready datasets, finding a modeled 2.29 pp margin increase in the margin-protection case versus baseline.
