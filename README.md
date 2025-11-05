# Housing Market Analysis

## Dashboard Link

https://app.powerbi.com/Redirect?action=OpenApp&appId=4be9bb63-8948-4390-9a4c-d2e75b5040ef&ctid=5e81dc77-db51-4a31-8660-6858117beb25&experience=power-bi

## Problem Statement

This dashboard provides insights into housing market performance across different cities, regions and house types. It helps understand purchase trends, price differences between offer & purchase values, inflation & interest impacts, and overall property valuation per square meter. Through interactive filters, decision-makers can analyze which location and property type offers the best investment yield.

### Steps Followed

 1. Imported the Housing dataset into Power BI Desktop.


 2. Performed Data Profiling in Power Query (Column Quality, Distribution & Profile) to identify missing values and data inconsistencies.


 3. Cleaned and transformed the data:

      ⦁	Removed null / duplicate entries

      ⦁	Standardized data types (Date, Numeric, Category)
	
      ⦁	Created derived columns where needed.



4. Created a Date Table and marked it as the Date Table for accurate time-based analysis.


5. Established relationships between tables and ensured correct model design.


6. Built DAX Measures for core KPIs including:

      ⦁	Average Offer Price

      ⦁	Average Purchase Price

      ⦁	Yield %

      ⦁	Interest Rate & Inflation

      ⦁	Price Per Sqm



7. Designed Page 2 – Market Overview with KPI Cards and trend visuals.


8. Designed Page 3 – Sales Performance with YTD, Monthly Trends and Regional Comparisons.


9. Designed Page 4 – House Type Analysis comparing Offer & Purchase Price, Yield, Interest, Inflation, Sqm & Sqm Price by House Type.


10. Added Slicers for:

      ⦁	Area

      ⦁	City

      ⦁	Sales Type

      ⦁	Region


11. Applied a professional theme ensuring consistent formatting and visuals.


12. Published the report to Power BI Service for secure sharing.


---




## DAX Measures & Logic

### Page 1 – Market Overview  

```DAX
Unit Sold in latest Year & Quater =
CALCULATE(
    DISTINCTCOUNT(Housing[house_id]),
    YEAR(Housing[date]) = YEAR(MAX(Housing[date])) &&
    QUARTER(Housing[date]) = QUARTER(MAX(Housing[date]))
)

Last 12 month sales =
CALCULATE(
    SUM(Housing[purchase_price]),
    DATESINPERIOD(Housing[date], MAX(Housing[date]), -12, MONTH)
)

   ``` 
![image alt](https://github.com/Vasanth-TD/Housing-data/blob/6b82437a2c0c533682d08fb339f64b1e9249074a/images/unit%20sold.png)


```DAX
Median Sales Price change =
VAR currentyearsales =
    MEDIANX(
        FILTER(Housing, YEAR(Housing[date].[Date]) = YEAR(MAX(Housing[date].[Date]))),
        Housing[purchase_price]
    )
VAR previousyearsales =
    MEDIANX(
        FILTER(Housing, YEAR(Housing[date].[Date]) = YEAR(MAX(Housing[date].[Date])) - 1),
        Housing[purchase_price]
    )
RETURN
    IF(previousyearsales <> 0,
       (currentyearsales - previousyearsales) / previousyearsales,
       BLANK())

Offer Price =
(100 * Housing[purchase_price]) /
(100 - Housing[%_change_between_offer_and_purchase])

  ```
![image alt](https://github.com/Vasanth-TD/Housing-data/blob/1385e79b4bbf826ebe9fd6232afebb4f8cd1e59e/images/page%201%20_d2.png)

  ```DAX
YOY sales =
VAR Currentyearsales =
    CALCULATE(
       SUM(Housing[purchase_price]),
       YEAR(Housing[date]) = YEAR(MAX(Housing[date]))
    )
VAR Previousyearsales =
    CALCULATE(
       SUM(Housing[purchase_price]),
       YEAR(Housing[date]) = YEAR(MAX(Housing[date])) - 1
    )
RETURN
    IF(Previousyearsales <> 0,
       (Currentyearsales - Previousyearsales) / Previousyearsales,
       BLANK())

   ```

   ![image alt](https://github.com/Vasanth-TD/Housing-data/blob/1385e79b4bbf826ebe9fd6232afebb4f8cd1e59e/images/page%201_d3.png)


### Page 2 – Sales Performance
```DAX
   Total YTD =
TOTALYTD(
    SUM(Housing[purchase_price]),
    Housing[date].[Date]
)

![image alt](https://github.com/Vasanth-TD/Housing-data/blob/1385e79b4bbf826ebe9fd6232afebb4f8cd1e59e/images/Page%202%20_d1.png)

```
```DAX
Last Month Sale =
CALCULATE(
    SUM(Housing[purchase_price]),
    DATESINPERIOD(
      Housing[date],
      MAX(Housing[date]),
      -1,
      MONTH
    )
)
```
![image alt](https://github.com/Vasanth-TD/Housing-data/blob/1385e79b4bbf826ebe9fd6232afebb4f8cd1e59e/images/page%202%20_d2.png)
```DAX
Average sqm Price =
AVERAGE(Housing[sqm_price])

Sales_by_Region =
CALCULATE(
    SUM(Housing[purchase_price]),
    ALLEXCEPT(Housing, Housing[region])
)
  ```
![image alt](https://github.com/Vasanth-TD/Housing-data/blob/1385e79b4bbf826ebe9fd6232afebb4f8cd1e59e/images/page%202%20_d3.png)

### Page 3 – House Type Analysis

#### Calculated visuals comparing
Average Offer Price & Purchase Price by House Type
Average Interest Rate, Yield & Inflation Rate by House Type
Average Sqm & Sqm Price by House Type
These calculations are built using aggregated visuals and summarization in the report view.
## 📸 Snapshots of Dashboard
![image alt](https://github.com/Vasanth-TD/Housing-data/blob/c4408bcfa4d067d2fc2c73d3bf8de6ba9bfb5963/images/PBS_1.png)

![image alt](https://github.com/Vasanth-TD/Housing-data/blob/c4408bcfa4d067d2fc2c73d3bf8de6ba9bfb5963/images/PBS_2.png)

![image alt](https://github.com/Vasanth-TD/Housing-data/blob/c4408bcfa4d067d2fc2c73d3bf8de6ba9bfb5963/images/PBS_3.png)

![image alt](https://github.com/Vasanth-TD/Housing-data/blob/c4408bcfa4d067d2fc2c73d3bf8de6ba9bfb5963/images/PBS_4.png)

## Insights & Recommendations
 Highest number of units sold in the latest quarter, showing strong recent activity.
### Price Trends 
Median sales price change shows the direction of market strength; an increase suggests rising demand or supply constraints.
### Offer vs Purchase Price 
Offer Price metric helps identify discounting or negotiation margins.
### Region Performance  
Some regions clearly outperform others in total sales and price per sqm—focus investment there.
###Buyer Demographics 
Age (via Key Influencers) shows younger/older buyers may pay differently or buy different house types.
### House Type Differentiation 
Villas and Farms may command higher area price and yield; Apartments may be entry-level with higher volumes.
###Space Efficiency 
Sqm & Sqm Price comparisons show which housing categories deliver more space for value.
### Action Opportunities 
Target promotional efforts in under-performing regions.
Adjust pricing strategy based on house type and buyer profile.
Monitor change in average sqm price as leading indicator of market shift.

Use offer/purchase spread to negotiate and evaluate deal profitabilit
