## <b> 📈 1. KPI & Aggregation Analysis

### <b> Q1 </b>

#### *Management wants a five-year performance summary. Calculate total revenue, total units sold, total purchase cost, total gross profit, and gross margin percentage.*

``` python

value_calculation = rice.copy()

value_calculation["Total_Sales"] = value_calculation["Per Unit Price (INR)"] * value_calculation["Unit Sold"]

value_calculation["Total_Purchase"] = value_calculation["Purchase Cost (INR)"] * value_calculation["Unit Sold"]

value_calculation["Profit"] = value_calculation["Total_Sales"] - value_calculation["Total_Purchase"]

a = value_calculation["Total_Sales"].sum()

b = value_calculation["Total_Purchase"].sum()

c = value_calculation["Profit"].sum()

d = value_calculation["Unit Sold"].sum()

e = (c / a) * 100
e = f"{e:.2f} %"

final_calculation = pd.Series(
    [a, d, b, c, e],
    index=["Total Revenue",
    "Total Units Sold",
    "Total Purchase Cost",
    "Total Gross Profit",
    "Gross Margin Percentages"]
)

final_calculation

```

## 📷 Output

![](Git_hub_Output/1.png)

---

### <b> Q2 </b>

#### *Which locations generated the highest total revenue during the five-year period?*

``` python

top_revenue = rice

column_seperation = top_revenue[["Year", "Location", "Unit Sold", "Per Unit Price (INR)"]]

column_seperation["Total_Sales"] = column_seperation["Per Unit Price (INR)"] * column_seperation["Unit Sold"]

store_finding = column_seperation.groupby(["Location"])["Total_Sales"].sum()

store_finding.sort_values(inplace= True, ascending= False)

store_finding.head(1)

```

## 📷 Output

![](Git_hub_Output/2.png)

---

### <b> Q3 </b>

#### *Compare the seven product categories based on revenue, units sold, gross profit, and gross margin.*

``` python

product_category = rice

product_category["Product Category"].unique()

product_category["Total_Sales"] = product_category["Per Unit Price (INR)"] * product_category["Unit Sold"]

product_category["Total_pur"] = product_category["Purchase Cost (INR)"] * product_category["Unit Sold"]

product_category["Profit"] = product_category["Total_Sales"] - product_category["Total_pur"]

pro_calculation = product_category.groupby("Product Category").agg({
    "Total_Sales" : "sum",
    "Unit Sold" : "sum",
    "Profit" : "sum",
})

pro_calculation.sort_values(by=["Total_Sales", "Profit"], inplace=True, ascending=False)

pro_calculation["Gross_Margin"] = (pro_calculation["Profit"] / pro_calculation["Total_Sales"]) * 100

pro_calculation["Gross_Margin"] = pro_calculation["Gross_Margin"].map(lambda x: f"{x :.2f} %")

pro_calculation

```

## 📷 Output

![](Git_hub_Output/3.png)

---

### <b> Q4 </b>

#### *Which months generated the highest and lowest total revenue?*

``` python

months_values = rice

months_values.head()

months_values["month_names"] = months_values["Date"].dt.month_name()

months_values["Total_Sales"] = months_values["Per Unit Price (INR)"] * months_values["Unit Sold"]

Values_seperation = months_values[["month_names", "Total_Sales"]]

month_result = Values_seperation.groupby("month_names")["Total_Sales"].sum()

month_result.sort_values(inplace=True, ascending=False)

month_result

```

## 📷 Output

![](Git_hub_Output/4.1.png)
![](Git_hub_Output/4.2.png)

---

### <b> Q5 </b>

#### *Which brands generated the highest total units sold?*

``` python

brand = rice
brand_calculation = brand.groupby("Rice Brand")["Unit Sold"].sum().sort_values(ascending= False)
brand_calculation

```

## 📷 Output

![](Git_hub_Output/5.png)

---

### <b> Q6 </b>

#### *Calculate the average revenue generated per product across the complete dataset.*

``` python

product_average = rice.copy()
product_average["Total_Revenue"] = product_average["Per Unit Price (INR)"] * product_average["Unit Sold"]
product_average.groupby("Product Name")["Total_Revenue"].mean().sort_values(ascending= False)

```

## 📷 Output

![](Git_hub_Output/6.png)

---

### <b> Q7 </b>

#### *Which locations have above-average revenue but below-average gross margin?*

``` python

avg_margin_cal = rice.copy()

avg_margin_cal["Revenue"] = avg_margin_cal["Per Unit Price (INR)"] * avg_margin_cal["Unit Sold"]

avg_margin_cal.head(3)

location_avg = avg_margin_cal.groupby("Location")["Revenue"].mean()

avg_calculation = pd.DataFrame(location_avg)

avg_calculation["Average"] = avg_calculation["Revenue"].mean()

avg_calculation["Avg_segment"] = avg_calculation["Revenue"].apply(
                                lambda x : "Above_Average" if x > 723.212855 else "Below_Average")

avg_calculation.sort_values(by="Avg_segment", inplace=True)

avg_calculation

location_margin = avg_margin_cal[["Location", "Per Unit Price (INR)", "Purchase Cost (INR)", "Unit Sold"]]

location_margin["Gross_Margin"] = (
    (location_margin["Per Unit Price (INR)"] * location_margin["Unit Sold"])
    - (location_margin["Purchase Cost (INR)"] * location_margin["Unit Sold"])
)

margin = location_margin.groupby("Location")["Gross_Margin"].mean()

margin_calculation = pd.DataFrame(margin)

margin_calculation["Margin_Average"] = margin_calculation["Gross_Margin"].mean()

margin_calculation["Margin_segment"] = margin_calculation["Gross_Margin"].apply(
                                                    lambda x : "Above_Average" if x > 126.444228 else "Below_Average")

margin_calculation.sort_values(by="Margin_segment", inplace=True)

margin_calculation

location_segment = pd.merge(avg_calculation, margin_calculation, on="Location", how="outer")

location_segment.sort_values(by="Avg_segment")

location_segment[(location_segment["Avg_segment"] == "Above_Average") & (location_segment["Margin_segment"] == "Below_Average")]

```

## 📷 Output

![](Git_hub_Output/7.png)

---

### <b> Q8 </b>

#### *For each package size, calculate revenue, units sold, gross profit, and average selling price.*

## <b> 📈 2. Conditional & Category-Based Analysis

### <b> Q9 </b>

#### *Classify products into High, Medium, and Low profitability based on gross margin. Compare the revenue generated by each group.*

### <b> Q10 </b>

#### *Identify products that have high sales volume but below-average gross margin.*

### <b> Q11 </b>

#### *Identify locations that have above-average revenue but below-average units sold.*

### <b> Q12 </b>

#### *Identify products whose selling price is at least 20% higher than their purchase cost.*

### <b> Q13 </b>

#### *Management wants products that satisfy both conditions:*

#### *• above-average units sold*

#### *• above-average gross profit*

### <b> Q14 </b>

#### *Identify brands that generate high revenue but have relatively weak profitability.*

## <b> 📈 3. Top / Bottom Performer Analysis

### <b> Q15 </b>

#### *Identify the top 10 products by total revenue.*

### <b> Q16 </b>

#### *Identify the top five brands by total gross profit.*

### <b> Q17 </b>

#### *For every product category, identify the top three products by revenue.*

### <b> Q18 </b>

#### *For every location, identify the top three products by units sold.*

### <b> Q19 </b>

#### *Identify the bottom 10 products by gross profit.*

### <b> Q20 </b>

#### *Identify the five locations with the lowest revenue.*

### <b> Q21 </b>

#### *Identify the top three brands within every location.*

### <b> Q22 </b>

#### *Which products rank among the top performers based on both revenue and gross profit?*

## <b> 📈 4. Period-over-Period Comparison

### <b> Q23 </b>

#### *Calculate month-over-month revenue growth for the entire business.*

#### *Identify the months with the largest increase and largest decline.*

### <b> Q24 </b>

#### *Compare each location's monthly revenue with its previous month and identify locations experiencing significant declines.*

### <b> Q25 </b>

#### *Calculate year-over-year revenue growth for each product category.*

### <b> Q26 </b>

#### *Identify product categories whose units sold declined compared with the previous month.*

### <b> Q27 </b>

#### *Identify products whose revenue declined for at least two consecutive months.*

### <b> Q28 </b>

#### *Identify locations that show a consistent improvement in revenue over multiple periods.*

### <b> Q29 </b>

#### *Compare the first year and the final year of the dataset. Which categories improved the most?*

### <b> Q30 </b>

#### *Identify products whose revenue increased while their units sold decreased between two comparable periods. Investigate what may have caused this.*

## <b> 📈 5. Trend & Cumulative Analysis

### <b> Q31 </b>

#### *Calculate cumulative company revenue month by month from January 2020 to December 2024.*

### <b> Q32 </b>

#### *Calculate cumulative gross profit for every location and compare long-term profitability.*

### <b> Q33 </b>

#### *Which product categories show a sustained upward revenue trend over the five-year period?*

### <b> Q34 </b>

#### *Which product categories show a sustained decline in units sold?*

### <b> Q35 </b>

#### *Determine whether each brand's long-term revenue trend is growing, declining, or relatively stable.*

### <b> Q36 </b>

#### *Identify products that have experienced a major long-term change in sales performance.*

### <b> Q37 </b>

#### *Analyse how the revenue contribution of each product category changed from the beginning to the end of the five-year period.*

## <b> 📈 6. Rolling / Moving-Window Analysis

### <b> Q38 </b>

#### *Calculate the three-month rolling average of total revenue and use it to understand the underlying sales trend.*

### <b> Q39 </b>

#### *Calculate a six-month rolling average of units sold for each product category.*

#### *Which categories show sustained weakness?*

### <b> Q40 </b>

#### *Calculate a three-month rolling revenue measure for every location.*

#### *Identify locations whose recent performance is below their normal trend.*

### <b> Q41 </b>

#### *Analyse the rolling gross profit of the major product categories and identify categories whose profitability is weakening.*

### <b> Q42 </b>

#### *Identify products whose recent three-month performance is significantly different from their longer-term performance.*

## <b> 📈 7. First & Latest Event Analysis

### <b> Q43 </b>

#### *For every product, identify its first recorded sales month and latest recorded sales month.*

### <b> Q44 </b>

#### *For each brand, compare revenue in its first recorded month with revenue in its latest recorded month.*

### <b> Q45 </b>

#### *For every location, determine the first and latest month in which sales were recorded.*

### <b> Q46 </b>

#### *For each product category, compare its performance during its first available period with its most recent period.*

### <b> Q47 </b>

#### *Identify products that appeared early in the dataset but have very weak or zero activity in later periods.*

## <b> 📈 8. Previous & Next Event Analysis

### <b> Q48 </b>

#### *Calculate the change in monthly revenue for every location compared with its previous month.*

### <b> Q49 </b>

#### *For every product, calculate the change in units sold from the previous month.*

#### *Identify unusually large changes.*

### <b> Q50 </b>

#### *Identify product-location combinations where sales suddenly increased or decreased compared with the previous month.*

### <b> Q51 </b>

#### *For each brand, calculate month-to-month revenue changes and identify periods of high volatility.*

### <b> Q52 </b>

#### *For every product, compare its current month with its previous month and identify the largest positive and negative movements.*

## <b> 📈 9. Duplicate & Record-Quality Analysis

### <b> Q53 </b>

#### *Check whether the dataset contains exact duplicate records.*

### <b> Q54 </b>

#### *Investigate whether multiple records exist for the same Date + Location + Product ID combination.*

#### *Determine whether those duplicates are legitimate or potentially problematic.*

### <b> Q55 </b>

#### *Check whether the same Product ID is associated with different product names, categories, brands, or package sizes.*

### <b> Q56 </b>

#### *Identify products whose selling price changes across different locations or months.*

#### *Determine whether this appears to be a legitimate business variation or a data-quality issue.*

### <b> Q57 </b>

#### *Investigate whether the same product appears under inconsistent naming conventions.*

#### *Explain how you would clean the data before analysis.*

## <b> 📈 10. Segmentation & Classification Analysis

### <b> Q58 </b>

#### *Divide locations into High-, Medium-, and Low-revenue segments. Compare the characteristics of each group.*

### <b> Q59 </b>

#### *Divide products into four groups using sales volume and gross margin:*

#### *• High volume / High margin*

#### *• High volume / Low margin*

#### *• Low volume / High margin*

#### *• Low volume / Low margin*

#### *Identify the products in each group.*

### <b> Q60 </b>

#### *Segment brands according to their contribution to total revenue and identify the strategically important brands.*

### <b> Q61 </b>

#### *Segment products based on their profitability and determine which segments deserve attention from management.*

### <b> Q62 </b>

#### *Segment package sizes based on their sales performance and determine which size has the strongest business contribution.*

## <b> 📈 11. Missing, Inactive & Gap Analysis

### <b> Q63 </b>

#### *Identify product-location combinations with zero sales in one or more months.*

### <b> Q64 </b>

#### *Identify products that had sales previously but later experienced multiple consecutive zero-sales months.*

### <b> Q65 </b>

#### *Find locations where particular categories became inactive for extended periods.*

### <b> Q66 </b>

#### *Identify product-location combinations with irregular sales activity and determine where further investigation is needed.*

### <b> Q67 </b>

#### *Identify months where one or more product categories had no recorded sales at a particular location.*

## <b> 📈 12. Cohort & Retention Analysis

### <b> Q68 </b>

#### *Identify each customer's first purchase month and create customer cohorts based on that month.*

### <b> Q69 </b>

#### *For customers acquired in each month, calculate how many returned and purchased again in the following month.*

### <b> Q70 </b>

#### *Calculate 30-day, 60-day, and 90-day customer retention.*

### <b> Q71 </b>

#### *Identify customers who made purchases in multiple months and calculate their repeat-purchase rate.*

### <b> Q72 </b>

#### *Compare high-value repeat customers with one-time customers in terms of revenue contribution.*

### <b> Q73 </b>

#### *Identify month-wise segments whose purchase frequency is increasing or declining over time.*

## <b> 📈 13. Relationship & Cross-Entity Analysis

### <b> Q74 </b>

#### *Analyse the relationship between brand and location. Identify brands whose performance is concentrated in only a few locations.*

### <b> Q75 </b>

#### *Analyse the relationship between category and package size. Identify the dominant package size for each category.*

### <b> Q76 </b>

#### *Determine whether high-revenue locations also tend to have high gross margins.*

### <b> Q77 </b>

#### *Determine whether the brands that sell the largest number of units are also the brands generating the largest revenue.*

### <b> Q78 </b>

#### *Analyse the relationship between selling price and units sold. Determine whether higher-priced products generally sell less.*

### <b> Q79 </b>

#### *Identify products that perform strongly in one location but poorly in another.*

### <b> Q80 </b>

#### *Identify categories that are highly dependent on a particular brand or package size.*

## <b> 📈 14. Contribution & Share Analysis

### <b> Q81 </b>

#### *Calculate each product category's percentage contribution to total company revenue.*

### <b> Q82 </b>

#### *Calculate each brand's percentage contribution to total revenue and total units sold. Compare the two.*

### <b> Q83 </b>

#### *Calculate every location's percentage contribution to company revenue.*

### <b> Q84 </b>

#### *For each month, determine the revenue contribution percentage of every product category.*

### <b> Q85 </b>

#### *Calculate what percentage of total revenue is generated by the top 10 products.*

### <b> Q86 </b>

#### *Calculate the percentage contribution of each location to total gross profit.*

### <b> Q87 </b>

#### *Identify categories whose share of total revenue is increasing over time.*

## <b> 📈 15. Root-Cause & Drill-Down Analysis

### <b> Q88 </b>

#### *Management says:*

#### *"Overall revenue has declined in some months. Find out why."*

#### *Investigate the issue from:*

#### *Company → Location → Category → Brand → Product*

#### *Then identify the major contributors to the decline.*
