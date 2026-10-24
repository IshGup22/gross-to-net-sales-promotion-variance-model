# Build the Power BI dashboard (Power BI Desktop)

**There is no native `.pbix` file in this package.** This kit provides the data, DAX, Excel model and visualization preview so you can create it in Power BI Desktop.

1. On Windows, open Power BI Desktop > **Get Data > Text/CSV**. Import `powerbi_monthly_financials.csv` and `powerbi_scenario_financials.csv`. Rename tables **Monthly** and **Scenarios**.
2. Under **Transform Data**, set `Month` and `Category` as Text, all GBP columns as Decimal Number. `ContributionMargin` and `DiscountRate` in Scenarios are decimals formatted Percentage; `MarginChange_pp` is already a point number, do not format as percent. Select **Close & Apply**.
3. **Modeling > New measure**: copy each measure from `POWER_BI_MEASURES.dax` into its own New Measure box. Do not relate Monthly and Scenarios (separate scenario summary).
4. Add four **Card** visuals: Gross Sales, Net Sales, Contribution Profit, Contribution Margin. Set GBP formats; display label as synthetic/demo.
5. Add a **Waterfall** chart using `variance_bridge.csv`: category = Driver and Y = Amount_GBP. IMPORTANT: the start/end total rows are totals; for an exact waterfall, use a table visual or explicitly set first/last as totals if supported.
6. Add **Clustered bar chart**: Axis = Scenarios[Scenario], Values = Scenarios[Contribution_GBP].
7. Add second **Clustered bar chart**: Axis = Scenarios[Scenario], Values = Scenarios[ContributionMargin].
8. Add **Line chart**: X = Monthly[Month], Y = Contribution Profit, and a slicer for Monthly[Category].
9. Add a **Matrix** with Rows = Monthly[Category], columns = Monthly[Month], Values = Net Sales and Contribution Variance.
10. Add a text box at bottom: *Synthetic transactions; cost, discount, trade spend and plan assumptions are illustrative*.
11. Save as `Gross_to_Net_Promotion_Dashboard.pbix`. Export an image/screenshot and put it at `images/powerbi_dashboard.png` in GitHub.

## On a Mac
Power BI Desktop is a Windows app. Use a Windows PC or supported virtual Windows setup. Alternatively build the same visuals in Excel and state clearly they are Excel dashboards, **not** Power BI.
