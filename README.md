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

avg_margin_cal["Total_Purchase"] = avg_margin_cal["Purchase Cost (INR)"] * avg_margin_cal["Unit Sold"]

avg_margin_cal["Profit"] = avg_margin_cal["Revenue"] - avg_margin_cal["Total_Purchase"]

avg_margin_cal.head(2)

avg_group = avg_margin_cal.groupby("Location")["Revenue"].sum()

avg_calculation = pd.DataFrame(avg_group)

avg_calculation["Revenue_Average"] = avg_calculation["Revenue"].mean()

avg_calculation["Revenue_Segment"] = avg_calculation["Revenue"].map(lambda x : "Above Average" if x > 967658 else "Below Average")

margin_group = avg_margin_cal.groupby("Location")["Profit"].sum()

mar_calculation = pd.DataFrame(margin_group)

margin_cal = pd.merge(avg_calculation, mar_calculation, on="Location")

margin_cal["Margin"] = (margin_cal["Profit"] / margin_cal["Revenue"]) * 100

margin_cal["Margin_Average"] = margin_cal["Margin"].mean()

margin_cal["Margin_Segment"] = margin_cal["Margin"].apply(lambda x : "Above_Margin" if x > 17.483498 else "Below_Margin")

margin_cal

margin_cal[(margin_cal["Revenue_Segment"] == "Above Average") & (margin_cal["Margin_Segment"] == "Below_Margin")]

```

## 📷 Output

![](Git_hub_Output/7.png)

---

### <b> Q8 </b>

#### *For each package size, calculate revenue, units sold, gross profit, and average selling price.*

``` python

package_size = avg_margin_cal.copy()

package_calculation = package_size.groupby("Product Quantity").agg({
    "Revenue" : "sum",
    "Unit Sold" : "sum",
    "Profit" : "sum",
})

package_calculation.rename(columns=
    {
        "Revenue" : "Total Revenue",
        "Unit Sold" : "Unit Sold",
        "Profi" : "Total Profit"
    }, inplace= True
)

package_calculation["average selling price"] = (package_calculation["Total Revenue"] / package_calculation["Unit Sold"]).round()

package_calculation

```

## 📷 Output

![](Git_hub_Output/8.png)

---

## <b> 📈 2. Conditional & Category-Based Analysis

### <b> Q9 </b>

#### *Classify products into High, Medium, and Low profitability based on gross margin. Compare the revenue generated by each group.*

``` python

hm1_profit = rice.copy()

hm1_profit.head(2)

pro_seperate = hm1_profit.groupby("Product Name")[["Sales", "Profit"]].sum()

pro_seperate["Gross_margin"] = (pro_seperate["Profit"] / pro_seperate["Sales"]) * 100

pro_seperate.sort_values(by=["Gross_margin"], ascending=False, inplace=True)

def prof(par):
    if par >= 17.6:
        return "High Profitability"

    elif par >= 17.4:
        return "Medium Profitability"

    else:
        return "Low Profitability"

pro_seperate["Profitability"] = pro_seperate["Gross_margin"].apply(prof)

pro_seperate

pro_seperate.groupby("Profitability")["Sales"].sum().sort_values(ascending=False)

```

## 📷 Output

![](Git_hub_Output/9.png)

---

### <b> Q10 </b>

#### *Identify products that have high sales volume but below-average gross margin.*

``` python

high_Sales = rice.copy()

average_cal = high_Sales.groupby("Product Name")[["Sales", "Unit Sold", "Profit"]].sum()

pro_average = average_cal["Sales"].mean().round()

pro_average

average_cal["Product_avg"] = np.where(average_cal["Sales"] > pro_average, "High_Sales", "Low_Sales")

average_cal.sort_values(by="Product_avg", inplace=True)

average_cal["Gross_Margin"] = (average_cal["Profit"] / average_cal["Sales"]) * 100

average_cal["Avg_Margin"] = average_cal["Gross_Margin"].mean()

average_cal["Margin_Segment"] = average_cal["Gross_Margin"].apply(lambda x : "Above Avg" if x > 17.483551 else "Below Avg")

average_cal[(average_cal["Product_avg"] == "High_Sales") & (average_cal["Margin_Segment"] == "Below Avg")]

```

## 📷 Output

![](Git_hub_Output/10.png)

---

### <b> Q11 </b>

#### *Identify locations that have above-average revenue but below-average units sold.*

``` python

location_avg_unit = rice.copy()

loc_sales_avg = location_avg_unit.groupby("Location")[["Sales", "Unit Sold"]].sum()

loc_sales_avg["Sales_Avg"] = loc_sales_avg["Sales"].mean()

loc_sales_avg["Sales_segment"] = loc_sales_avg["Sales"].apply(lambda x : "Above Avg" if x > 9676588.0 else "Below Avg")

loc_sales_avg["Unit_sold_Avg"] = loc_sales_avg["Unit Sold"].mean()

loc_sales_avg["Sold_segment"] = loc_sales_avg["Unit Sold"].apply(lambda x : "Above Avg" if x > 29368.764706 else "Below Avg")

loc_sales_avg[(loc_sales_avg["Sales_segment"] == "Above Avg") & (loc_sales_avg["Sold_segment"] == "Below Avg")]

```

## 📷 Output

![](Git_hub_Output/11.png)

---

### <b> Q12 </b>

#### *Identify products whose selling price is at least 20% higher than their purchase cost.*

``` python

hike_20 = rice

get_unique = hike_20[["Product Name", "Product Quantity", "Purchase Cost", "Unit Price"]]

get_unique = get_unique.drop_duplicates(subset=["Product Name", "Product Quantity"])

get_unique["twenty_cal"] = get_unique["Purchase Cost"] + ((get_unique["Purchase Cost"] / 100) * 20)

get_unique["twenty_logics"] = get_unique["Unit Price"] - get_unique["twenty_cal"]

get_unique["twenty_segment"] = get_unique["twenty_logics"].apply(lambda x : "Higher" if x >= 0 else "Lower")

get_unique[get_unique["twenty_segment"] == "Higher"]

```

## 📷 Output

![](Git_hub_Output/12.png)

---

### <b> Q13 </b>

#### *Management wants products that satisfy both conditions:*

#### *• above-average units sold*

#### *• above-average gross profit*

``` python

both_con = rice.copy()

unit_pro = both_con.groupby("Product Name")[["Unit Sold", "Profit", "Sales"]].sum()

unit_pro["Unit_avg"] = unit_pro["Unit Sold"].mean().round()

unit_pro["Unit_segment"] = unit_pro["Unit Sold"].apply(lambda x : "Above Avg" if x > unit_pro["Unit Sold"].mean() else "Below Avg")

unit_pro["profi_avg"] = unit_pro["Profit"].mean().round()

unit_pro["Profit_segment"] = unit_pro["Profit"].apply(lambda x : "Above Avg" if x > unit_pro["Profit"].mean() else "Below Avg")

unit_pro[(unit_pro["Unit_segment"] == "Above Avg") & (unit_pro["Profit_segment"] == "Above Avg")]

```

## 📷 Output

![](Git_hub_Output/13.png)

---

### <b> Q14 </b>

#### *Identify brands that generate high revenue but have relatively weak profitability.*

``` python

high_week = rice

avg_cal = high_week.groupby("Rice Brand")[["Sales", "Profit"]].sum()

avg_cal["Sales"].mean()

avg_cal["Sales_segment"] = np.where(avg_cal["Sales"] > avg_cal["Sales"].mean(), "High_Revenue", "Low_Revenue")

avg_cal["Gross_margin"] = (avg_cal["Profit"] / avg_cal["Sales"]) * 100

avg_cal["Gross_margin"].mean()

avg_cal["profit_segment"] = np.where(avg_cal["Gross_margin"] > avg_cal["Gross_margin"].mean(), "High_Profit", "Week_Profit")

avg_cal[(avg_cal["Sales_segment"] == "High_Revenue") & (avg_cal["profit_segment"] == "Week_Profit")]

```

## 📷 Output

![](Git_hub_Output/14.png)

---

## <b> 📈 3. Top / Bottom Performer Analysis

### <b> Q15 </b>

#### *Identify the top 10 products by total revenue.*

``` python

top_10 = rice.copy()

top_10.groupby("Product Name")["Sales"].sum().sort_values(ascending= False).head(10)

```

## 📷 Output

![](Git_hub_Output/15.png)

---

### <b> Q16 </b>

#### *Identify the top five brands by total gross profit.*

``` python

top_5_profit = rice.groupby("Rice Brand")["Profit"].sum().sort_values(ascending= False).head(5)

top_5_profit

```

## 📷 Output

![](Git_hub_Output/16.png)

---

### <b> Q17 </b>

#### *For every product category, identify the top three products by revenue.*

``` python

top_3 = rice.copy()

first_way = top_3.groupby(["Product Category", "Product Name"])["Sales"].sum().groupby(level= 0).nlargest(3)

```

## 📷 Output

![](Git_hub_Output/17.1.png)

``` python

sec_way = top_3.groupby(["Product Category", "Product Name"])["Sales"].sum().reset_index()

sec_way.sort_values(by= ["Product Category", "Sales"], ascending= False).groupby("Product Category").head(3)

```

## 📷 Output

![](Git_hub_Output/17.2.png)

---

### <b> Q18 </b>

#### *For every location, identify the top three products by units sold.*

``` python

top_3_unit = rice.copy()

top_3_unit.groupby(["Location", "Product Name"])["Unit Sold"].sum().groupby(level= 0).nlargest(3)

top_3_sec_app = top_3_unit.groupby(["Location", "Product Name"])["Unit Sold"].sum().reset_index()

top_3_sec_app.sort_values(by= ["Location", "Unit Sold"], ascending= False).groupby("Location").head(3)

```

## 📷 Output

![](Git_hub_Output/18.png)

---

### <b> Q19 </b>

#### *Identify the bottom 10 products by gross profit.*

``` python

rice.groupby("Product Name")["Profit"].sum().sort_values(ascending= False).tail(10)

```

## 📷 Output

![](Git_hub_Output/19.png)

---

### <b> Q20 </b>

#### *Identify the five locations with the lowest revenue.*

``` python

rice.groupby("Location")["Sales"].sum().sort_values(ascending= True).head(5)

```

## 📷 Output

![](Git_hub_Output/20.png)

---

### <b> Q21 </b>

#### *Identify the top three brands within every location.*

``` python

top_3_brand = rice.copy()

rice.groupby(["Location", "Rice Brand"])["Sales"].sum().groupby(level= 0).nlargest(3)

```

## 📷 Output

![](Git_hub_Output/21.png)

---

### <b> Q22 </b>

#### *Which products rank among the top performers based on both revenue and gross profit?*

``` python

rev_pro = rice.copy()

avg_cal_1 = rev_pro.groupby("Product Name")[["Sales", "Profit"]].sum()

avg_cal_1["Sales_Avg"] = np.where(avg_cal_1["Sales"] > avg_cal_1["Sales"].mean(), "top_Performance", "Not")

avg_cal_1["Profit_Avg"] = np.where(avg_cal_1["Profit"] > avg_cal_1["Profit"].mean(), "top_Performance", "Not")

value_sort = avg_cal_1[ (avg_cal_1["Sales_Avg"] == "top_Performance") & (avg_cal_1["Profit_Avg"] == "top_Performance")]

value_sort.sort_values(by= ["Sales", "Profit"],ascending= False)

```

## 📷 Output

![](Git_hub_Output/22.png)

---

## <b> 📈 4. Period-over-Period Comparison

### <b> Q23 </b>

#### *Calculate month-over-month revenue growth for the entire business.*

#### *Identify the months with the largest increase and largest decline.*

``` python

mom_com = rice.copy()

mom_com["Month_name"] = mom_com["Date"].dt.month_name()

month_cal = mom_com.groupby(["Year", "Month_name", "Month"])[["Sales"]].sum()

month_cal.sort_values(by=["Year", "Month"], inplace=True)

month_cal["Month_shift"] = month_cal["Sales"].shift(1)

month_cal["MOM"] = ((month_cal["Sales"] - month_cal["Month_shift"]) / month_cal["Month_shift"]) * 100

```

## 📷 Output

![](Git_hub_Output/23.png)

---

### <b> Q24 </b>

#### *Compare each location's monthly revenue with its previous month and identify locations experiencing significant declines.*

``` python

sig_dec = rice.copy()

sig_dec["Month Name"] = sig_dec["Date"].dt.month_name()

val_group = sig_dec.groupby(["Location", "Year", "Month Name", "Month"])["Sales"].sum()

val_frame = pd.DataFrame(val_group)

val_frame = val_frame.sort_values(by=["Location", "Year", "Month"])

val_frame["Previous Month"] = val_frame.groupby("Location")["Sales"].shift(1)

val_frame["pre_mon_Diff"] = val_frame["Sales"] - val_frame["Previous Month"]

val_frame["pre_mon_segment"] = val_frame["pre_mon_Diff"].apply(lambda x : "significant declines" if x < -50000 else "Not")

val_frame[val_frame["pre_mon_segment"] == "significant declines"]

```

## 📷 Output

![](Git_hub_Output/24.png)

---

### <b> Q25 </b>

#### *Calculate year-over-year revenue growth for each product category.*

``` python

yoy_frame = rice.copy()

yoy_cal = pd.DataFrame(yoy_frame.groupby(["Product Category", "Year"])["Sales"].sum())

yoy_cal["pre_year_com"] = yoy_cal.groupby("Product Category")["Sales"].shift(1)

yoy_cal["pre_year_diff"] = ((yoy_cal["Sales"] - yoy_cal["pre_year_com"]) / yoy_cal["pre_year_com"]) * 100

yoy_cal["YOY"] = yoy_cal["pre_year_diff"].map(lambda x : f"{x :.2f} %")

yoy_cal

```

## 📷 Output

![](Git_hub_Output/25.png)

---

### <b> Q26 </b>

#### *Identify product categories whose units sold declined compared with the previous month.*

``` python

pro_cat_dec = rice.copy()

pro_cat_dec["Month_name"] = pro_cat_dec["Date"].dt.month_name()

pro_frame = pd.DataFrame(pro_cat_dec.groupby(["Product Category", "Year", "Month_name", "Month"])["Unit Sold"].sum())

pro_cal = pro_frame.sort_values(by=["Product Category", "Year", "Month"])

pro_cal["Pre_Month"] = pro_cal.groupby("Product Category")["Unit Sold"].shift(1)

pro_cal["month_diff"] = pro_cal["Unit Sold"] - pro_cal["Pre_Month"]

pro_cal[pro_cal["month_diff"] < 0]

```

## 📷 Output

![](Git_hub_Output/26.png)

---

### <b> Q27 </b>

#### *Identify products whose revenue declined for at least two consecutive months.*

``` python

two_con = rice.copy()

two_con["month_name"] = two_con["Date"].dt.month_name()

two_con_frame = pd.DataFrame(two_con.groupby(["Product Name", "Year", "month_name", "Month"])["Sales"].sum())

two_con_gro = two_con_frame.sort_values(by=["Product Name", "Year", "Month"])

two_con_gro["pre_month"] = two_con_gro.groupby("Product Name")["Sales"].shift(1)

two_con_gro["difference"] = two_con_gro["Sales"] - two_con_gro["pre_month"]

two_con_gro["Diff_shift"] = two_con_gro.groupby("Product Name")["difference"].shift(1)

two_con_gro[(two_con_gro["difference"] < 0) & (two_con_gro["Diff_shift"] < 0)]

```

## 📷 Output

![](Git_hub_Output/27.png)

---

### <b> Q28 </b>

#### *Identify locations that show a consistent improvement in revenue over multiple periods.*

``` python

con_imp = rice.copy()

con_imp["Month_name"] = con_imp["Date"].dt.month_name()

con_imp_frame = pd.DataFrame(con_imp.groupby(["Location", "Year"])["Sales"].sum())

con_imp_frame["Pre_Year"] = con_imp_frame.groupby("Location")["Sales"].shift(1)

con_imp_frame["Difference"] = con_imp_frame["Sales"] - con_imp_frame["Pre_Year"]

con_imp_frame["shift_Diff"] = con_imp_frame.groupby("Location")["Difference"].shift(1)

con_imp_frame[(con_imp_frame["Difference"] > 0) & (con_imp_frame["shift_Diff"] > 0)]

```

## 📷 Output

![](Git_hub_Output/28.png)

---

### <b> Q29 </b>

#### *Compare the first year and the final year of the dataset. Which categories improved the most?*

``` python

first_last = rice.copy()

fir_las_frame = pd.DataFrame(first_last.groupby(["Product Category", "Year"])["Sales"].sum())

fir_las_com = fir_las_frame.sort_values(by=["Product Category", "Year"], ascending=True)

fir_las_com["shift"] = fir_las_com.groupby("Product Category")["Sales"].shift(4)

fir_las_com["Diff"] = fir_las_com["Sales"] - fir_las_com["shift"]

fir_las_com.sort_values(by="Diff", ascending=False).head(1)

```

## 📷 Output

![](Git_hub_Output/29.png)

---

### <b> Q30 </b>

#### *Identify products whose revenue increased while their units sold decreased between two comparable periods. Investigate what may have caused this.*

``` python

inc_dec = rice.copy()

inc_dec["Month name"] = inc_dec["Date"].dt.month_name()

inc_dec_frame = pd.DataFrame(inc_dec.groupby(["Product Name", "Year", "Month name", "Month"])[["Sales", "Unit Sold"]].sum())

inc_dec_frame = inc_dec_frame.sort_values(by=["Product Name", "Year", "Month"])

inc_dec_frame["Pre_mon_Sales"] = inc_dec_frame.groupby("Product Name")["Sales"].shift(1)

inc_dec_frame["sales_diff"] = inc_dec_frame["Sales"] - inc_dec_frame["Pre_mon_Sales"]

inc_dec_frame["Pre_mon_unit"] = inc_dec_frame.groupby("Product Name")["Unit Sold"].shift(1)

inc_dec_frame["unit_diff"] = inc_dec_frame["Unit Sold"] - inc_dec_frame["Pre_mon_unit"]

inc_dec_frame[(inc_dec_frame["unit_diff"] < 0) & (inc_dec_frame["sales_diff"] > 0)]

```

## 📷 Output

![](Git_hub_Output/30.png)

---

## <b> 📈 5. Trend & Cumulative Analysis

### <b> Q31 </b>

#### *Calculate cumulative company revenue month by month from January 2020 to December 2024.*

``` python

month_cumulative = rice.copy()

month_cum_Frame = pd.DataFrame(month_cumulative.groupby(["Year", "Month_Name", "Month"])["Sales"].sum())

month_cum_Frame = month_cum_Frame.sort_values(by=["Year", "Month"])

month_cum_Frame["cumulative"] = month_cum_Frame["Sales"].cumsum()

month_cum_Frame.head(25)

```

## 📷 Output

![](Git_hub_Output/31.png)

---

### <b> Q32 </b>

#### *Calculate cumulative gross profit for every location and compare long-term profitability.*

``` python

loc_cum = rice.copy()

loc_cum_frame = pd.DataFrame(loc_cum.groupby(["Location", "Year", "Month_Name", "Month"])["Profit"].sum())

loc_cum_frame = loc_cum_frame.sort_values(by=["Location", "Year", "Month"])

loc_cum_frame["Cumulative"] = loc_cum_frame.groupby("Location")["Profit"].cumsum()

value_filter = loc_cum_frame.xs((2024, "December"), level=("Year", "Month_Name"))

value_filter = value_filter.sort_values(by=["Cumulative"], ascending=False)

value_filter

```

## 📷 Output

![](Git_hub_Output/32.png)

---

### <b> Q33 </b>

#### *Which product categories show a sustained upward revenue trend over the five-year period?*

``` python

year_sus = rice.copy()

year_sus_fra = pd.DataFrame(year_sus.groupby(["Product Category", "Year"])["Sales"].sum())

year_sus_fra["previous_year"] = year_sus_fra.groupby("Product Category")["Sales"].shift(1)

year_sus_fra["Diff"] = year_sus_fra["Sales"] - year_sus_fra["previous_year"]

year_sus_fra["Diff"] = year_sus_fra["Sales"] - year_sus_fra["previous_year"]

filter_val = year_sus_fra.groupby("Product Category")["Diff"].apply(lambda x : x.dropna().gt(0).all())

fil_index = filter_val[filter_val].index

year_sus_fra = year_sus_fra.reset_index()

year_sus_fra[year_sus_fra["Product Category"].isin(fil_index)]

```

## 📷 Output

![](Git_hub_Output/33.png)

---

### <b> Q34 </b>

#### *Which product categories show a sustained decline in units sold?*

``` python

unit_sus = rice.copy()

unit_frame = pd.DataFrame(unit_sus.groupby(["Product Category", "Year"])["Unit Sold"].sum())

unit_frame = unit_frame.sort_values(by=["Product Category", "Year"])

unit_frame["pre year"] = unit_frame.groupby("Product Category")["Unit Sold"].shift(1)

unit_frame["Diff"] = unit_frame["Unit Sold"] - unit_frame["pre year"]

frame_filter = unit_frame.groupby(["Product Category"])["Diff"].apply(lambda x : x.dropna().lt(0).all())

frame_filter[frame_filter].index

unit_frame = unit_frame.reset_index()

unit_frame[unit_frame["Product Category"].isin(frame_filter)]

```

## 📷 Output

![](Git_hub_Output/34.png)

---

### <b> Q35 </b>

#### *Determine whether each brand's long-term revenue trend is growing, declining, or relatively stable.*

``` python

gro_dec_sta = rice.copy()

gro_frame = pd.DataFrame(gro_dec_sta.groupby(["Rice Brand", "Year"])["Sales"].sum())

gro_frame = gro_frame.sort_values(by=["Rice Brand", "Year"])

gro_frame["Pre_year"] = gro_frame.groupby("Rice Brand")["Sales"].shift(1)

gro_frame["Growth"] = ((gro_frame["Sales"] - gro_frame["Pre_year"]) / gro_frame["Pre_year"]) * 100

gro_frame["Growth"] = gro_frame["Growth"].apply(lambda x : f"{x :.2f} %")

gro_frame

```

## 📷 Output

![](Git_hub_Output/35.png)

---

### <b> Q36 </b>

#### *Identify products that have experienced a major long-term change in sales performance.*

``` python

long_term = rice.copy()

long_term_frame = pd.DataFrame(long_term.groupby(["Product Name", "Year"])["Sales"].sum())

long_term_frame["Pre Year"] = long_term_frame.groupby("Product Name")["Sales"].shift(1)

long_term_frame["Diff"] = ((long_term_frame["Sales"] - long_term_frame["Pre Year"]) / long_term_frame["Pre Year"]) * 100

long_calculation = long_term_frame.groupby("Product Name")["Diff"].agg(["min", "mean", "max"])

long_calculation

```

## 📷 Output

![](Git_hub_Output/36.png)

---

### <b> Q37 </b>

#### *Analyse how the revenue contribution of each product category changed from the beginning to the end of the five-year period.*

``` python

five_year_per = rice.copy()

five_frame = pd.DataFrame(five_year_per.groupby(["Year", "Product Category"])["Sales"].sum())

year_sum = five_frame.groupby("Year")["Sales"].sum()

year_sum = year_sum.reset_index()

five_frame = five_frame.reset_index()

five_frame["Total_sales"] = five_frame["Year"].map(year_sum.set_index("Year")["Sales"])

five_frame["contribution"] = (five_frame["Sales"] / five_frame["Total_sales"]) * 100

five_frame["contribution"] = five_frame["contribution"].apply(lambda x: f"{x :.2f} %")

five_frame

```

## 📷 Output

![](Git_hub_Output/37.png)

---

## <b> 📈 6. Rolling / Moving-Window Analysis

### <b> Q38 </b>

#### *Calculate the three-month rolling average of total revenue and use it to understand the underlying sales trend.*

```python

rolling_3 = rice.copy()

roll_frame = pd.DataFrame(rolling_3.groupby(["Year", "Month_Name", "Month"])["Sales"].sum())

roll_frame = roll_frame.sort_values(by=["Year", "Month"])

roll_frame["3_mon_roll"] = roll_frame["Sales"].rolling(3).mean()

pd.set_option("display.float_format", "{:,.0f}".format)

```

## 📷 Output

![](Git_hub_Output/38.png)

---

### <b> Q39 </b>

#### *Calculate a six-month rolling average of units sold for each product category.*

```python

six_mon_avg = rice.copy()

six_frame = six_mon_avg.groupby(["Product Category", "Year", "Month", "Month_Name"])["Unit Sold"].sum()

six_frame = six_frame.reset_index()

six_frame = six_frame.sort_values(by=["Product Category", "Year", "Month"])

six_frame["roll_avg"] = six_frame.groupby("Product Category")["Unit Sold"].transform(lambda x : x.rolling(6).mean())

six_frame["diff"] = six_frame.groupby("Product Category")["roll_avg"].shift(1)

six_frame["diff_cal"] = six_frame["roll_avg"] - six_frame["diff"]

six_frame["diff_cal_shift"] = six_frame.groupby("Product Category")["diff_cal"].shift(1)

sus_week = six_frame[(six_frame["diff_cal"] < 0) & (six_frame["diff_cal_shift"] < 0)]

```

## 📷 Output

![](Git_hub_Output/39.png)

---

#### *Which categories show sustained weakness?*

### <b> Q40 </b>

#### *Calculate a three-month rolling revenue measure for every location.*

#### *Identify locations whose recent performance is below their normal trend.*

```python

loc_avg = rice.copy()

loc_frame = pd.DataFrame(loc_avg.groupby(["Location", "Year", "Month", "Month_Name"])["Sales"].sum())

loc_frame = loc_frame.sort_values(by=["Location", "Year", "Month"])

loc_frame["rolling_3"] = loc_frame.groupby("Location")["Sales"].transform(lambda x : x.rolling(3).mean().round())

loc_frame["shift_1"] = loc_frame.groupby("Location")["rolling_3"].shift(1)

loc_frame["diff"] = ((loc_frame["rolling_3"] - loc_frame["shift_1"]) / loc_frame["shift_1"]) * 100

loc_avg_total = loc_frame.groupby("Location")["diff"].mean()

loc_avg_total = loc_avg_total.reset_index()

loc_frame = loc_frame.reset_index()

loc_frame["loc_overall_avg"] = loc_frame["Location"].map(loc_avg_total.set_index("Location")["diff"])

result_cal = loc_frame[(loc_frame["Year"] == 2024) & (loc_frame["Month"] >= 10)]

result_cal["avg_segment"] = result_cal.apply(
    lambda row : "High_Performance" if row["diff"] > row["loc_overall_avg"] else "Low_Performance", axis=1
)

```

## 📷 Output

![](Git_hub_Output/40.png)

---

### <b> Q41 </b>

#### *Analyse the rolling gross profit of the major product categories and identify categories whose profitability is weakening.*

```python

pro_3 = rice.copy()

pro_frame = pd.DataFrame(pro_3.groupby(["Product Category", "Year", "Month", "Month_Name"])["Profit"].sum())

pro_frame = pro_frame.sort_values(by=["Product Category", "Year", "Month"])

pro_frame["roll_3"] = pro_frame.groupby("Product Category")["Profit"].transform(lambda x : x.rolling(3).mean().round())

year_avg = pd.DataFrame(pro_frame.groupby(["Product Category", "Year"])["roll_3"].mean().round())

year_avg["pro_shift"] = year_avg.groupby("Product Category")["roll_3"].shift(1)

year_avg["diff"] = year_avg["roll_3"] - year_avg["pro_shift"]

year_avg

```

## 📷 Output

![](Git_hub_Output/41.png)

---

### <b> Q42 </b>

#### *Identify products whose recent three-month performance is significantly different from their longer-term performance.*

```python

long_term = rice.copy()

long_Frame = pd.DataFrame(long_term.groupby(["Product Name", "Year", "Month", "Month_Name"])["Sales"].sum())

long_Frame = long_Frame.sort_values(by=["Product Name", "Year", "Month"])

long_Frame["roll_avg"] = long_Frame.groupby("Product Name")["Sales"].transform(lambda x : x.rolling(3).mean().round())

total_avg = long_Frame.groupby("Product Name")["roll_avg"].mean().round()

total_avg = total_avg.reset_index()

long_Frame = long_Frame.reset_index()

long_Frame["pro_avg"] = long_Frame["Product Name"].map(total_avg.set_index("Product Name")["roll_avg"])

value_filter = long_Frame[(long_Frame["Year"] == 2024) & (long_Frame["Month"].isin([12]))].copy()

value_filter["growth"] = (((value_filter["roll_avg"] - value_filter["pro_avg"]) / value_filter["pro_avg"]) * 100).round()

positive_growth = value_filter[value_filter["growth"] >= 10]

positive_growth

```

## 📷 Output

![](Git_hub_Output/42.png)

---

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
