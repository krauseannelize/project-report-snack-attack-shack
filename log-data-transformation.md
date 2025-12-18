# Data Transformation Log

This log documents the steps taken in the Google Sheets data analysis project.

## 2025-04-16 - Initial Analysis

- **Data Source:**
  - Location: [data-snack-attack-shack-challenge](https://docs.google.com/spreadsheets/d/17sne5-vAbKo69kr0YPQMmApfRUFgpff48aJP6NSLnRY/edit?usp=drive_link)
  - Description: Customers, products and orders information
  - Sheets:
    - 1 orders
    - 2 products
    - 3 customers

- **Data Processing:**
    - Step 1: Create a copy of the workbook with name `analysis-snack-attack-shack-challenge`.
    - Step 2: Uploaded the workbook to AI for initial exploration and querying. *For details on AI interactions, refer to the* [AI Usage Log](/snack-attack-shack/log-ai-usage.md).

## 2025-04-17 - Data Processing & Analysis

  - Step 3: Create a copy of the "1 orders" worksheet and rename the copy to "analysis-revenue".
  - Step 4: Change the named table name to "revenue".
  - Step 5: Delete columns "requiredDate", "shippedDate", "arrivedDate", "freightCost" and "shipVia".
  - Step 6: Insert column "customerName" and use VLOOKUP to pull in the customer name from the "3 customers" worksheet.
  - Step 7: Insert column "customerCity" and use VLOOKUP to pull in the customer city from the "3 customers" worksheet.
  - Step 8: Decide to omit customer region from the analysis. *Rationale: 60 of the 91 customers have NULL in this field*
  - Step 9: Insert column "customerCountry" and use VLOOKUP to pull in the customer country from the "3 customers" worksheet.
  - Step 10: Insert column "productName" and use VLOOKUP to pull in the product name from the "2 products" worksheet.
  - Step 11: Insert column "productCategory" and use VLOOKUP to pull in the category name from the "2 products" worksheet.
  - Step 12: Insert column "productCost" and use a combination of VLOOKUP and YEAR to pull in product price from the "purchasePrice_2022", "purchasePrice_2023" and "purchasePrice_2024" columns in the "2 products" worksheet, respectively.
  - Step 13: Insert column "productMarkup" to calculate the percentage markup from productCost to orderPrice.
  - Step 14: Insert column "orderTotal" to calculate total of order multiplying order price and quantity before discount.
  - Step 15: Insert column "revenue" to calculate the amount paid by the customer by deducting any discounts from the order total.
  - Step 16: Insert column "profit" to calculate the profit by deducting total product cost (multiply product cost and quantity) from revenue.
  - Step 17: Insert column "profitMargin" to calculate the percentage profit over revenue.
  - Step 18: Apply a filter to "orderDate" to only show transactions before 2024-05-01. *Rationale: Only partial records for May 2024 transactions exist, which will skew results.*
  - Step 19: Create a new worksheet named "summary" to consolidate key metrics and streamline analysis.
  - Step 20: Extracted a list of all customers and their associated revenue using the formula: `=QUERY(revenue,"SELECT C, SUM(P) WHERE C IS NOT NULL GROUP BY C ORDER BY SUM(P) DESC LABEL C 'Customer', SUM(P) 'Revenue'", 1)`.
  - Step 21: Inserted a column "% of Total" to calculate each customer's percentage of the total revenue.
  - Step 22: Inserted a column "Cumulative" to calculate a running balance of the "% of Total," providing insight into cumulative revenue distribution per customer.

## 2025-04-18 - Continued Data Processing & Analysis

  - Step 23: Create a new worksheet named "visualizations" to centralize charts and graphs for analysis.
  - Step 24: Created a combination chart titled "Pareto Chart: Revenue by Customers" with:
    - Columns: Representing customer revenue.
    - Line: Representing cumulative % of total revenue.
  - Step 25: Extracted a list of all products and their associated revenue in the "summary" worksheet by using the formula: `=QUERY(revenue,"SELECT H, SUM(P) WHERE H IS NOT NULL GROUP BY H ORDER BY SUM(P) DESC LABEL H 'Product', SUM(P) 'Revenue'", 1)`.
  - Step 26: Inserted a column "% of Total" to calculate each products's percentage of the total revenue.
  - Step 27: Inserted a column "Cumulative" to calculate a running balance of the "% of Total," providing insight into cumulative revenue distribution per product.
  - Step 28: Created a combination chart in "visualizations" titled "Pareto Chart: Revenue by Products" with:
    - Columns: Representing product revenue.
    - Line: Representing cumulative % of total revenue.
  - Step 29: Extracted a list of all categories and their associated revenue in the "summary" worksheet by using the formula: `=QUERY(revenue,"SELECT I, SUM(P) WHERE I IS NOT NULL GROUP BY I ORDER BY SUM(P) DESC LABEL I 'Category', SUM(P) 'Revenue'", 1)`.
  - Step 30: Inserted a column "% of Total" to calculate each category's percentage of the total revenue.
  - Step 31: Inserted a column "Cumulative" to calculate a running balance of the "% of Total," providing insight into cumulative revenue distribution per category.
  - Step 32: Created a combination chart in "visualizations" titled "Pareto Chart: Revenue by Category" with:
    - Columns: Representing category revenue.
    - Line: Representing cumulative % of total revenue.
  - Step 33: Added comments to cells containing the QUERY function to warn users: *"The QUERY function references specific columns. If columns in the source data are added, removed, or reordered, this formula may break or return incorrect results. Update the QUERY parameters as needed."*
  - Step 34: Delete rows for orders placed in May 2024 and updated values accordingly in report. *Rationale: Noticed the filter applied in the "analysis-revenue" worksheet didn't exclude the transactions from initial analyses. As May 2024 only has partial records, it is excluded for ensure consistency and reliability.*
  - Step 35: Extracted a list of all countries and their associated revenue in the "summary" worksheet by using the formula: `=QUERY(revenue,"SELECT E, SUM(P) WHERE E IS NOT NULL GROUP BY E ORDER BY SUM(P) DESC LABEL E 'Country', SUM(P) 'Revenue'", 1)`. Included comments with warning - see Step 33.
  - Step 36: Inserted a column "% of Total" to calculate each country's percentage of the total revenue.
  - Step 37: Inserted a column "Cumulative" to calculate a running balance of the "% of Total," providing insight into cumulative revenue distribution per country.
  - Step 38: Created a combination chart in "visualizations" titled "Pareto Chart: Revenue by Country" with:
    - Columns: Representing country revenue.
    - Line: Representing cumulative % of total revenue.
  - Step 39: Created a heat map visualization representing country revenue on a world map in "visualizations". Use AI to choose an accessible color scheme. Incorporate gradient colors: Pale Gold (#FFE699), Orange (#FFA500), and Royal Blue (#002FA7) for low, mid, and high values, respectively.
  - Step 40: Inserted a pivot table in "summary" referencing the "analysis-revenue" worksheet with the following configuration:
    - Rows:
      - customerCountry (sorted descending by SUM of revenue)
      - customerCity (sorted descending by SUM of revenue).
    - Values:
      - Revenue: SUM (default).
      - Revenue: SUM as % of grand total.
    - Filter: Applied filter on customerCountry to analyze city-level contributions per selected country.
  - Step 41: Create a copy of the "analysis-revenue" worksheet and rename the copy to "analysis-dynamic".
  - Step 42: Change the named table name to "dynamic".
  - Step 43: Delete "customerCity" and "customerCountry" columns.
  - Step 44: Added a new column titled "orderCount" to track the cumulative count of unique orders per customer using the formula `=COUNTUNIQUEIFS($A$2:$A2, $B$2:$B2, B2)`. This calculates a running balance of distinct orders per customer, ensuring line items for the same order are counted only once.
  - Step 45: Added a new column titled "sinceLastOrder" to calculate the days since the customer's last order using the formula `=IF(E2=1, 0, D2 - INDEX($D$2:$D2,MATCH(1, ($B$2:$B2=B2)*($E$2:$E2=E2-1), 0)))`. This will help to identify gaps in customer order history for classification purposes.
  - Step 46: Added a new column titled "customerStatus" to classify customers as New, Non-Returning, Dormant, or Regular based on their order history and purchasing patterns. The classification criteria is:
    - New: Customer placed exactly 1 order within the 180 days preceding 2024-04-30, the last day of our range.
    - Non-Returning: Customer has placed exactly 1 order and the gap between their order date and 2024-04-30 exceeds 180 days.
    - Dormant: Customer has placed multiple orders, but has not made a purchase within the last 180 days and at least one gap between orders exceed 365 days.
    - Regular: Customer has placed multiple orders, all gaps between orders are less than or equal to 365 days, with at least one purchase within the 180 days preceding 2024-04-30.
    The classification was done using this formula:
    `=IF(MAXIFS(dynamic[orderCount], dynamic[customerID], B2)=1, 
       IF((DATE(2024,4,30) - MAXIFS(dynamic[orderDate], dynamic[customerID], B2)) > 180, "Non-Returning", "New"), 
       IF(AND(MAX(dynamic[sinceLastOrder]) > 365, (DATE(2024,4,30) - MAXIFS(dynamic[orderDate], dynamic[customerID], B2)) > 180), "Dormant", "Regular"))`

- **Data Correction:**
  - Corrected customer "Que Delícia" city from " 12Rio de Janeiro" to "Rio de Janeiro" in the original "3 customers" worksheet to ensure data accuracy.

## 2025-04-19 - Continued Data Processing & Anaysis

  - Step 47: Inserted a pivot table in "summary" referencing the "analysis-dynamic" worksheet with the following configuration:
    - Rows:
      - customerStatus (sorted ascending by customerStatus)
      - customerName (sorted descending by Total Revenue).
    - Values:
      - Total Revenue: SUM of revenue.
      - Order Count: MAX of orderCount.
      - AVG Revenue: Calculated field to calculate the average revenue per number of orders with formula: `=SUM(revenue)/MAX(orderCount)`.
      - Frequency: Calculated field to calculate the order frequency by getting the average days between order with the formula: `=ROUND(SUM(sinceLastOrder)/IF(MAX(orderCount)=1,1,MAX(orderCount)-1))`. I subtract 1 to exclude the first order that does not have a gap.
      - Elapsed: Calculated field to calculate the time that has elapsed since customer's last order and the end of range 30 April 2024 with formula: `=DATE(2024,4,30) - MAX(orderDate)`.
  - Step 48: Refined the "customerStatus" column logic to improve classification accuracy and expand the scope of analysis. Adjustments included:
    - New: Customers are now classified as "New" if their first order occurred within the 120 days preceding 2024-04-30, regardless of how many orders they've placed.
    - Non-Returning: Expanded the criteria to include customers with up to 2 orders, where their most recent order occurred more than 180 days before 2024-04-30.
    - Dormant: Adjusted the definition of "multiple orders" to require at least 3 orders.
    - Regular: Adjusted the definition of "multiple orders" to require at least 3 orders.
    The classification was done using this formula:
    `=IF(MINIFS(dynamic[orderDate], dynamic[customerID], B2) >= DATE(2024,4,30)-120, 
       "New", 
       IF(MAXIFS(dynamic[orderCount], dynamic[customerID], B2)<=2, 
          IF((DATE(2024,4,30) - MAXIFS(dynamic[orderDate], dynamic[customerID], B2)) > 180, "Non-Returning", "Dormant"), 
        IF(AND(MAX(dynamic[sinceLastOrder]) > 365, (DATE(2024,4,30) - MAXIFS(dynamic[orderDate], dynamic[customerID], B2)) > 180), "Dormant", "Regular")))`
  - Step 49: Inserted a new column "productStatus" in the "analysis-dynamic" worksheet looking up the status in the "2 products" worksheet and assigning "Discontinued" and "Current".
  - Step 50: Created a pivot table in the "summary" worksheet with the following configuration:
    - Rows:
      - productStatus (sorted ascending by productStatus).
      - productName (sorted descending by SUM of revenue).
   - Values:
     - Order Count: COUNTUNIQUE of orderID.
     - Total Revenue: SUM of revenue.
     - Last Order: MAX of orderDate.
  - Step 51: Created a second pivot table in the "summary" worksheet with the following configuration:
    - Rows:
      - productName (sorted ascending by productName).
      - Month: orderDate (sorted ascending by orderDate and grouped by year-month).
    - Values:
      - Total Revenue: SUM of revenue.
    - Filter: Applied a filter to productName, to focus individually on each of the four discontinued products in the top 25 products by revenue (e.g., Alice Mutton).
  - Step 52: Copied the filtered pivot table three times to generate individual tables for each of the remaining products (Thüringer Rostbratwurst, Rössle Sauerkraut, and Perth Pasties).
  - Step 53: Created area graphs for each of the four discontinued products in the "visualizations" worksheet. Each graph includes a moving average trendline to illustrate changes in revenue patterns over time.
  - Step 54: Created a pivot table in the "summary" worksheet with the following configuration:
    - Rows:
      - orderPrice sorted in ascending order and grouped into intervals of 10 (e.g., €0.00-€10.00, €10.00-€20.00, etc.).
    - Values:
      - Order Count: SUM of orderQuantity.
      - Total Revenue: SUM of revenue.
  - Step 55: Used the resulting pivot table to generate a graph in the "visualizations" worksheet:
    - Graph Type: A dual-axis graph with:
      - Left Y-Axis: Order Count.
      - Right Y-Axis: Total Revenue.
      - X-Axis: Price brackets (orderPrice) grouped in intervals of 10.
  - Step 56: Added two moving average trendlines to the graph:
    - Trendline indicating relationship between Order Count and price brackets.
    - Trendline indicating relationship between Total Revenue and price brackets.

## 2025-04-20 - Continued Data Processing & Anaysis

- Step 57: Created a copy of "1 orders" worksheet and renamed it "analysis-operations". - Changed the named table name to "operations".
- Step 58: eleted columns "customerID", "orderPrice" and "discountApllied" - Insert column "productName" and use VLOOKUP to pull in the product name from the "2 products" worksheet.
- Step 59: Inserted a new column "productStatus" in the "analysis-dynamic" worksheet looking up the status in the "2 products" worksheet and assigning "Discontinued" and "Current". - Inserted a new column "unitsStocked" and used VLOOKUP to pull in the units in stock from the "2 products" worksheet.
- Step 60: Inserted a new column "unitsOrdered" and used VLOOKUP to pull in the units on order from the "2 products" worksheet.
- Step 61: Inserted a new column "reorderLevel" and used VLOOKUP to pull in the reorder level from the "2 products" worksheet.
- Step 62: Added a new column titled "orderCount" to track the cumulative count of unique orders per customer using the formula `=COUNTUNIQUEIFS($A$2:$A2,$F$2:$F2,F2)`. This calculates a running balance of distinct orders per customer, ensuring line items for the same order are counted only once.
- Step 63: Added a new column titled "sinceLastOrder" to calculate the days since the customer's last order using the formula `=IF(M2=1, 0, B2 - INDEX($B$2:$B2,MATCH(1, ($F$2:$F2=F2)*($M$2:$M2=M2-1), 0)))`. This will help to identify gaps in customer order history for classification purposes.
- Step 64: Added a new column titled "salesVelocity" to measure the average rate of product sales over time using the formula: `=IF(operations[sinceLastOrder]=0, 0, operations[orderQuantity] / operations[sinceLastOrder])`. This calculates the velocity by dividing the order quantity by the days since the last order providing insight into how quickly products are selling and helps identify stockout risks or slow-moving inventory.
- Step 65: Created a pivot table in the "summary" worksheet with the following configuration:
    - Rows:
      - productStatus (sorted ascending by productStatus).
      - productName (sorted ascending by Days-to-Stockout).
   - Values:
     - In Stock: MAX of unitsStocked
     - Ordered: MAX of unitsOrdered
     - Reorder Level: MAX of reorderLevel
     - Days-to-Stockout: Add a calculated field with the formula: `=unitsStocked / AVERAGE(salesVelocity)` to estimate the number of days until stock depletion.
     - Levels: Add a calculated field with the formula: `=IF(unitsStocked > (AVERAGE(salesVelocity) * 30), "Excess", "Normal")` to flag products with excess inventory relative to sales velocity.
     - Status: Add a calculated field with the formula: `=IF(unitsStocked > reorderLevel, "Adequate", IF((unitsStocked + unitsOrdered) > reorderLevel, "On Order", "Reorder"))` to classify products based on their stock levels and reorder thresholds.
- Step 66: Add new column "deliveryDuration" to "analysis-operations" worksheet to calculate the time from shipping to arrival.
- Step 67: Created a pivot table in the "summary" worksheet with the following configuration:
    - Rows:
      - Carrier: Displays the name of each shipping carrier (sorted ascending by Average Duration).
    - Values:
      - Average Duration: AVERAGE of deliveryDuration.
      - Cost Effectiveness: Add a calculated field with the formula: `=AVERAGE(freightCost) / AVERAGE(shipDuration)` to evaluate the cost per shipment for each carrier.
- Step 68: Used the resulting pivot table to generate a graph in the "visualizations" worksheet to visually compare shipping carriers based on both speed and cost-effectiveness, enabling quick identification of optimal carriers:
    - Graph Type: A dual axis combination chart.
    - Setup:
      - X-Axis: Carrier names.
      - Left Y-Axis: Average Delivery Duration displayed as bars.
      - Right Y-Axis: Average Freight Cost (€) displayed as a line overlay.
- Step 69: Revised the original question "Sales Trends" to "Profitability Trends" using AI assistance, ensuring alignment with the analysis focus.
- Step 70: Created a pivot table in the "summary" worksheet with the following configuration:
    - Rows:
      - orderDate: Sorted ascending and grouped by Year-Month.
    - Values:
      - Profit: SUM of profit.
      - Profit Margin: AVERAGE of profitMargin.
- Step 71: Used the resulting pivot table to generate a graph in the "visualizations" worksheet to illustrate monthly profit and profit margin trends from July 2022 to April 2024:
    - Graph Type: A dual-axis graph with:
      - Left Y-Axis: Profit (Area graph).
      - Right Y-Axis: Profit Margin (Line graph).
      - X-Axis: orderDate in Year-Month format.
- Step 72: Created a pivot table in the "summary" worksheet to summarize sales volume data by month and year for trend analysis:
    - Rows:
      - orderDate: Sorted ascending and grouped by Year-Month.
    - Values:
      - Sales Volume: SUM of orderQuantity.
- Step 73: Used the resulting pivot table to create a graph in the "visualizations" worksheet showing sales volume trends over time from July 2022 to April 2024
    - Graph Type: An area graph with:
      - Y-Axis: Sales Volume.
      - X-Axis: orderDate in Year-Month format.
- Step 74: Added a new column in the "analysis-revenue" worksheet named "orderCosts" with the formula: `=revenue[productCost] * revenue[orderQuantity]` to calculate total product costs for each order.
- Step 75: Created a pivot table in the "summary" worksheet with the following configuration:
    - Rows:
      - orderDate: Sorted ascending and grouped by Year-Month.
    - Values:
      - Product Cost: SUM of orderCosts.
      - Sale Price: SUM of orderTotal.
- Step 76: Used the resulting pivot table to create a graph in the "visualizations" worksheet showing product cost and sale price trends over time:
    - Graph Type: A stacked area chart with:
      - Y-Axis: Product Cost & Sale Price.
      - X-Axis: orderDate in Year-Month format.
- Step 77: Added a new table to "summary" worksheet by copying the sale volume pivot and extending it with forecasted values for May to July 2024 using the formula: `=FORECAST(BC25,$BD$3:$BD$24,$BC$3:$BC$24)`
- Step 78: Used the resulting table to create a graph in the "visualizations" worksheet showing sales volume trend over time adding a 3-month forecast:
    - Graph Type: A column chart with:
      - Y-Axis: Sales Volume.
      - X-Axis: orderDate in Year-Month format.
- Step 79: Added a new table to "summary" worksheet by copying the product cost (and sale price) pivot and extending it with forecasted values for May to July 2024 using the formula: `=FORECAST(BF25,$BG$3:$BG$24,$BF$3:$BF$24)`
- Step 80: Used the resulting table to create a graph in the "visualizations" worksheet showing product cost trend over time adding a 3-month forecast:
    - Graph Type: A column chart with:
      - Y-Axis: Product Cost.
      - X-Axis: orderDate in Year-Month format.

## 2025-04-22 - Fixes

- **Fixes**
  - Discover all values in column "orderQuantity" in worksheet "analysis-operations" has been replaced with 12. I use the formula: `=INDEX('1 orders'!I2:I, MATCH(1, ('1 orders'!A2:A=A2)*('1 orders'!G2:G=F2), 0))` to look up the correct order quantity from the original "1 Orders" worksheet.
  - Update analysis and conclusions.
