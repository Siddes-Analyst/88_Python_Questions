### <b>Q1. Five-Year Performance Summary:</b>

#### *Management wants a five-year performance summary. Calculate total revenue, total units sold, total purchase cost, total gross profit, and gross margin percentage.*

```python
# Select required columns
rice_values = rice[["Date", "Per Unit Price (INR)", "Purchase Cost (INR)", "Unit Sold"]]

# Set Date as index
rice_values.set_index("Date", inplace=True)

# Rename columns
rice_values = rice_values.rename(columns={
    "Purchase Cost (INR)": "Purchase Cost",
    "Unit Sold": "Quantity",
    "Per Unit Price (INR)": "Sales"
})

# Calculate total sales and purchase cost
rice_values["Total_Sales"] = (
    rice_values["Sales"] * rice_values["Quantity"]
)

rice_values["Total_Purchase"] = (
    rice_values["Purchase Cost"] * rice_values["Quantity"]
)

# Calculate profit
rice_values["Profit"] = (
    rice_values["Total_Sales"] - rice_values["Total_Purchase"]
)

# Calculate yearly summary
gross_value = rice_values.resample("YE").sum()

# Calculate gross margin percentage
gross_value["Margin Percentage"] = (
    gross_value["Profit"] / gross_value["Total_Sales"]
) * 100

# Format margin percentage
gross_value["Margin Percentage"] = gross_value["Margin Percentage"].map(
    lambda x: f"{x:.2f} %"
)

gross_value
