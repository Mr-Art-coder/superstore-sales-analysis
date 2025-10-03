# 📊 Superstore Sales Data Analysis Report
## Table of Contents
- [Introduction](#introduction)
- [Problem Statement](#problem-statement)
- [Data Sourcing](#data-sourcing)
- [Data Transformation](#data-transformation)
- [Data Analysis and Visualization](#data-analysis-and-visualization)
- [Findings](#findings)
- [Recommendations](#recommendations)
- [Author](#author)

---

## 📌 Introduction

This project analyzes the **Superstore Sales Dataset**, a popular retail dataset containing detailed transactional records of customer orders. The dataset covers order details, shipping information, product categories, customer segments, and financial metrics such as sales, discount, and profit.

The goal of this analysis is to extract insights into:

* Sales performance
* Discount impact on profitability
* Regional and customer segmentation trends
* Shipping and delivery efficiency

Using **Excel** for data transformation, visualization, and KPI dashboards, this project simulates a real-world retail analytics workflow and generates actionable business recommendations.

---

## 🎯 Problem Statement

The analysis focuses on answering key business questions:

1. **Order Insights**

   * Total orders, orders by ship mode, segment, category, order date, ship date, state, region, city, sub-category, and product.
   * Identify top 10 customers by orders.

2. **Performance by Metrics**

   * Distribution of **quantity, customers, sales, profit, and discounts** across the same dimensions.

3. **Discount & Profitability**

   * Compare sales with vs. without discounts.
   * Identify *high discount – low profit* products using scatter/bubble charts.
   * Determine discount thresholds where profit turns negative.

4. **Delivery Performance**

   * Measure the difference between **Order Date** and **Ship Date** (delivery days).

---

## 📂 Data Sourcing

Dataset: [Superstore Dataset (Final) – Kaggle](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)

**Size**: ~9,800 rows × 21 columns
**Coverage**:

* **Orders**: Order ID, Order Date, Ship Date, Ship Mode
* **Customer**: Customer ID, Name, Segment, Region, State, City
* **Product**: Category, Sub-Category, Product ID, Product Name
* **Financials**: Sales, Quantity, Discount, Profit

---

## 🛠️ Data Transformation

Data was prepared using **Power Query in Excel**.

### Transformation Steps

| Step | Description                                                     |
| ---- | --------------------------------------------------------------- |
| 1    | Promoted headers from first row                                 |
| 2    | Assigned correct data types (text, date, number)                |
| 3    | Recalculated Discount as `Discount × Sales`                     |
| 4    | Reorganized and renamed columns                                 |
| 5    | Created `Delivery Days` = Ship Date – Order Date                |
| 6    | Added `"Discount Availability"` column (Discount / No Discount) |

### Power Query Script (M Code)

```m
let 
    Source = Csv.Document(File.Contents("C:\Users\AKOREDE\Documents\archive\Sample - Superstore.csv"),[Delimiter=",", Columns=21, Encoding=1252, QuoteStyle=QuoteStyle.None]), 
    #"Promoted Headers" = Table.PromoteHeaders(Source, [PromoteAllScalars=true]), 
    #"Changed Type" = Table.TransformColumnTypes(#"Promoted Headers",{
        {"Row ID", Int64.Type}, {"Order ID", type text}, {"Order Date", type date}, {"Ship Date", type date}, 
        {"Ship Mode", type text}, {"Customer ID", type text}, {"Customer Name", type text}, {"Segment", type text}, 
        {"Country", type text}, {"City", type text}, {"State", type text}, {"Postal Code", Int64.Type}, 
        {"Region", type text}, {"Product ID", type text}, {"Category", type text}, {"Sub-Category", type text}, 
        {"Product Name", type text}, {"Sales", type number}, {"Quantity", Int64.Type}, {"Discount", type number}, 
        {"Profit", type number}}), 
    #"Added Custom" = Table.AddColumn(#"Changed Type", "Discountt", each [Discount]*[Sales]), 
    #"Removed Columns" = Table.RemoveColumns(#"Added Custom",{"Discount"}), 
    #"Reordered Columns" = Table.ReorderColumns(#"Removed Columns",{"Row ID", "Order ID", "Order Date", "Ship Date", "Ship Mode", "Customer ID", "Customer Name", "Segment", "Country", "City", "State", "Postal Code", "Region", "Product ID", "Category", "Sub-Category", "Product Name", "Sales", "Quantity", "Discountt", "Profit"}), 
    #"Changed Type1" = Table.TransformColumnTypes(#"Reordered Columns",{{"Discountt", type number}}), 
    #"Renamed Columns" = Table.RenameColumns(#"Changed Type1",{{"Discountt", "Discount"}}), 
    #"Added Custom1" = Table.AddColumn(#"Renamed Columns", "Delivery Date", each [Ship Date]-[Order Date]), 
    #"Changed Type2" = Table.TransformColumnTypes(#"Added Custom1",{{"Delivery Date", type number}}), 
    #"Renamed Columns1" = Table.RenameColumns(#"Changed Type2",{{"Delivery Date", "Delivery Days"}}), 
    #"Added Conditional Column" = Table.AddColumn(#"Renamed Columns1", "Discount Availability", each if [Discount] = 0 then "No Discount" else "Discount") 
in 
    #"Added Conditional Column"
```

---

## 📊 Data Analysis and Visualization

### KPIs

* **Total Orders**
* **Total Sales**
* **Total Profit**
* **Total Discount**
* **Total Quantity Sold**
* **Unique Customers**
* **Orders by Ship Mode**

### Dashboard Visuals

* 📈 **Line Chart** – Trends in Sales, Profit, and Orders over time
* 📊 **Bar/Column Charts** – Orders, Profit by Category, Sub-Category, Region
* 🗺️ **Map Visualization** – Sales distribution by State/Region
* ⚪ **Scatter/Bubble Chart** – Discount vs Profit
* ⏱️ **Timeline & Slicers** – Interactive filters for Year, Segment, Category


---

## 📑 Findings

* **Discounts**: High discounts directly reduce profit; low discounts yield better margins.
* **Delivery Days**: Most orders delivered within **1–7 days**, indicating strong logistics.
* **Category Profitability**: *Furniture* contributes the highest profit.
* **Regional Analysis**: The **West region** drives the highest sales and profit.
* **Customer Segments**: *Consumers* are the top segment by orders, sales, and profit.
* **Geographic Gaps**: Southern cities contribute the least across all metrics.
* **High Discount – Low Profit Products**: Certain product lines lose profitability under aggressive discounts.

---

## ✅ Recommendations

1. **Optimize Discounts**

   * Reduce blanket discounting; target discounts only for loyal customers.
   * Identify discount thresholds where profit turns negative and avoid crossing them.

2. **Category Strategy**

   * Expand the **Furniture** product line due to high profitability.
   * Reassess product categories with high discounts but low profits.

3. **Regional Growth**

   * Double down on operations in the **West region**.
   * Develop marketing and pricing strategies for **Southern cities**.

4. **Customer Engagement**

   * Strengthen loyalty programs for *Consumers* (largest segment).
   * Retain high-value customers (Top 10 by orders/sales).

5. **Operational Excellence**

   * Maintain efficient shipping times (1–7 days).
   * Explore **same-day or next-day shipping** in high-order states for competitive advantage.

---

## ⚡ Author  

**👤 Name**: *Adegunle Raphael Tobi*  

**🐦 X**: <a href="https://x.com/AdegunleRT" target="_blank">X Profile</a>  

**🔗 LinkedIn**: <a href="https://www.linkedin.com/in/adegunleraphael/" target="_blank">LinkedIn Profile</a>  

**✉️ Email Me**: <a href="mailto:adegunleraphael@gmail.com" target="_blank">Email</a>  
  

