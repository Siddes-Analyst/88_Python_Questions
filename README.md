### <b> Q1. Five-Year Performance Summary:</b>

#### *Management wants a five-year performance summary. Calculate total revenue, total units sold, total purchase cost, total gross profit, and gross margin percentage.*

```python

value_calculation["Total_Sales"] = value_calculation["Per Unit Price (INR)"] * value_calculation["Unit Sold"]

value_calculation["Total_Purchase"] = value_calculation["Purchase Cost (INR)"] * value_calculation["Unit Sold"]

value_calculation["Profit"] = value_calculation["Total_Sales"] - value_calculation["Total_Purchase"]

a = value_calculation["Total_Sales"].sum()
a

b = value_calculation["Total_Purchase"].sum()
b

c = value_calculation["Profit"].sum()
c

d = value_calculation["Unit Sold"].sum()
d

e = (c / a) * 100
e = f"{e:.2f} %"
e

final_calculation = pd.Series(
    [a, d, b, c, e],
    index=["Total Revenue", "Total Units Sold", "Total Purchase Cost", "Total Gross Profit", "Gross Margin Percentages"]
)

final_calculation

```
![](https://github.com/Siddes-Analyst/88_Python_Questions/blob/main/Screenshot%202026-10-06%20115845.png)
