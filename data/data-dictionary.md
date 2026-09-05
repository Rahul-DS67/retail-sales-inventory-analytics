# 📘 Data Dictionary — Retail Sales & Inventory Analytics

## 1. Dataset Overview

This dataset was created for a retail analytics and business intelligence project focused on analyzing:

* Sales performance
* Product and category performance
* Store and regional performance
* Inventory availability
* Stockout patterns
* Promotional impact
* Holiday demand
* Competitor pricing
* Demand forecasting

The dataset contains **weekly retail sales and inventory observations** across multiple stores, regions, products, and categories for **2022–2023**.

> **Note:** The complete dataset is not included in this repository. This data dictionary provides the structure, definitions, and business meaning of the fields used in the analysis.

---

## 2. Dataset Coverage

| Attribute            | Details                |
| -------------------- | ---------------------- |
| Analysis Period      | 2022–2023              |
| Number of Stores     | 10                     |
| Number of Regions    | 5                      |
| Number of Products   | 25                     |
| Number of Categories | 5                      |
| Data Frequency       | Weekly                 |
| Data Grain           | Store × Product × Week |

### Product Categories

The dataset contains the following product categories:

* Electronics
* Grocery
* Apparel
* Home & Garden
* Sports

---

# 3. Data Structure

The primary dataset contains information covering four major analytical areas:

### Sales

Used to measure revenue, units sold, trends, and product/store performance.

### Inventory

Used to monitor inventory levels, utilization, availability, and stockout risk.

### Business Drivers

Used to evaluate promotions, holidays, and competitor pricing.

### Forecasting

Used to compare expected demand against actual sales.

---

# 4. Field Definitions

## 4.1 Time Fields

| Field          | Data Type    | Description                                 | Business Use                            |
| -------------- | ------------ | ------------------------------------------- | --------------------------------------- |
| `date`         | Date         | Date representing the weekly observation    | Time-based analysis and trend reporting |
| `week_of_year` | Integer      | Week number associated with the observation | Weekly trend and seasonality analysis   |
| `year`         | Integer      | Calendar year of the observation            | Year-over-year analysis                 |
| `month`        | Text/Integer | Month associated with the observation       | Monthly trend analysis                  |

### Notes

The `date` field is used as the primary time reference for the analysis.

A dedicated date dimension was created in Power BI to support time-based calculations and filtering.

---

# 4.2 Store & Geographic Fields

| Field      | Data Type    | Description                                 | Business Use                     |
| ---------- | ------------ | ------------------------------------------- | -------------------------------- |
| `store_id` | Text/Integer | Unique identifier for each store            | Store-level performance analysis |
| `region`   | Text         | Geographic region associated with the store | Regional performance comparison  |

The dataset contains **10 stores across 5 regions**.

These fields allow performance to be analyzed at both store and regional levels.

---

# 4.3 Product Fields

| Field          | Data Type    | Description                        | Business Use                        |
| -------------- | ------------ | ---------------------------------- | ----------------------------------- |
| `product_id`   | Text/Integer | Unique identifier for each product | Product-level analysis              |
| `product_name` | Text         | Name of the product                | Product performance reporting       |
| `category`     | Text         | Product category                   | Category-level performance analysis |

The dataset contains **25 products across 5 categories**.

---

# 4.4 Pricing Fields

| Field              | Data Type | Description                         | Business Use                 |
| ------------------ | --------- | ----------------------------------- | ---------------------------- |
| `unit_price`       | Decimal   | Selling price per unit              | Revenue and pricing analysis |
| `competitor_price` | Decimal   | Comparable competitor selling price | Competitive pricing analysis |

### Derived Pricing Analysis

The difference between the company's selling price and competitor price can be evaluated using:

**Price Difference = Unit Price − Competitor Price**

This helps assess whether products are priced above, below, or close to competitor pricing.

---

# 4.5 Sales Fields

| Field            | Data Type | Description                                   | Business Use                               |
| ---------------- | --------- | --------------------------------------------- | ------------------------------------------ |
| `sales_quantity` | Integer   | Number of units sold during the weekly period | Sales volume analysis                      |
| `revenue`        | Decimal   | Sales revenue generated during the period     | Revenue and financial performance analysis |

### Revenue Calculation

Where revenue is derived from sales quantity and selling price:

**Revenue = Sales Quantity × Unit Price**

In Power BI, revenue can be calculated using a DAX measure such as:

```text
Total Revenue =
SUMX(
    fact_sales,
    fact_sales[sales_quantity] * fact_sales[unit_price]
)
```

The exact implementation may vary depending on the final Power BI data model.

---

# 4.6 Inventory Fields

| Field             | Data Type | Description                                               | Business Use                             |
| ----------------- | --------- | --------------------------------------------------------- | ---------------------------------------- |
| `inventory_start` | Integer   | Inventory available at the beginning of the weekly period | Beginning inventory analysis             |
| `inventory_end`   | Integer   | Inventory remaining at the end of the weekly period       | Inventory availability and risk analysis |

These fields support analysis of inventory movement and stock availability.

### Inventory Utilization

A general inventory utilization concept can be evaluated by comparing units sold with available inventory.

Depending on the business definition, available inventory may be represented using beginning inventory, average inventory, or another defined inventory base.

---

# 4.7 Stockout Field

| Field           | Data Type         | Description                                             | Business Use                           |
| --------------- | ----------------- | ------------------------------------------------------- | -------------------------------------- |
| `stockout_flag` | Integer / Boolean | Indicates whether a stockout occurred during the period | Stockout monitoring and inventory risk |

### Flag Definition

```text
0 = No stockout
1 = Stockout occurred
```

The field is used to identify stores, products, categories, and regions experiencing inventory availability problems.

---

# 4.8 Promotion Field

| Field            | Data Type         | Description                                                                           | Business Use                     |
| ---------------- | ----------------- | ------------------------------------------------------------------------------------- | -------------------------------- |
| `promotion_flag` | Integer / Boolean | Indicates whether the product/store combination was under promotion during the period | Promotion effectiveness analysis |

### Flag Definition

```text
0 = No promotion
1 = Promotion active
```

The dataset contains both promotional and non-promotional observations, allowing sales performance to be compared between the two conditions.

---

# 4.9 Holiday Field

| Field          | Data Type         | Description                                                        | Business Use            |
| -------------- | ----------------- | ------------------------------------------------------------------ | ----------------------- |
| `holiday_flag` | Integer / Boolean | Indicates whether the observation occurred during a holiday period | Holiday demand analysis |

### Flag Definition

```text
0 = Non-holiday period
1 = Holiday period
```

This field helps identify changes in demand associated with holiday periods.

---

# 4.10 Forecast Field

| Field      | Data Type       | Description                                                 | Business Use                            |
| ---------- | --------------- | ----------------------------------------------------------- | --------------------------------------- |
| `forecast` | Integer/Decimal | Forecasted sales quantity for the corresponding observation | Demand planning and forecast evaluation |

The forecast field is compared with actual sales quantity to evaluate forecast performance.

---

# 5. Flag Fields

Three binary indicators are used to identify important business conditions.

| Flag             | 0            | 1                 |
| ---------------- | ------------ | ----------------- |
| `promotion_flag` | No promotion | Promotion active  |
| `holiday_flag`   | Non-holiday  | Holiday period    |
| `stockout_flag`  | No stockout  | Stockout occurred |

These flags enable comparative analysis between different business conditions.

---

# 6. Analytical Metrics

Several business metrics were developed from the raw dataset.

## Total Units Sold

Measures the total number of units sold during the selected period.

**Concept:**

```text
Total Units Sold = SUM(Sales Quantity)
```

---

## Total Revenue

Measures total sales revenue.

**Concept:**

```text
Total Revenue = SUM(Sales Quantity × Unit Price)
```

---

## Average Weekly Sales

Measures the average number of units sold per distinct week.

**Concept:**

```text
Average Weekly Sales =
Total Units Sold ÷ Number of Distinct Weeks
```

---

## Stockout Rate

Measures the proportion of observations associated with stockout conditions.

The exact denominator depends on the business definition used in the report.

---

## Promotion Revenue

Measures revenue generated during promotional periods.

---

## Promotion Lift %

Measures the relative improvement in performance associated with promotional periods compared with a defined non-promotional baseline.

---

## Forecast Error

Measures the difference between actual sales and forecasted sales.

**Concept:**

```text
Forecast Error =
Actual Sales − Forecast Sales
```

---

## Absolute Forecast Error

Measures forecast deviation without considering the direction of the error.

**Concept:**

```text
Absolute Forecast Error =
ABS(Actual Sales − Forecast Sales)
```

This is useful for identifying products, stores, regions, or periods where demand forecasts deviate substantially from actual sales.

---

# 7. Power BI Data Model

The Power BI solution uses a dimensional/star-schema approach.

### Main Tables

```text
                 ┌──────────────┐
                 │   dim_date   │
                 └──────┬───────┘
                        │
                        │
┌────────────────┐      │      ┌────────────────┐
│  dim_product   │──────┼──────│   dim_store    │
└────────────────┘      │      └────────────────┘
                        │
                        ▼
                 ┌──────────────┐
                 │  fact_sales  │
                 └──────────────┘
```

### Fact Table

**`fact_sales`**

Contains the weekly sales, inventory, pricing, promotion, holiday, stockout, and forecast observations.

### Dimension Tables

**`dim_date`**

Provides time attributes used for filtering and time-based analysis.

**`dim_product`**

Contains product-related attributes such as product ID, product name, and category.

**`dim_store`**

Contains store and regional attributes.

---

# 8. Data Relationships

The model uses relationships between the fact table and dimensions to support filtering and aggregation.

Conceptually:

```text
dim_date
   │
   └──────► fact_sales

dim_product
   │
   └──────► fact_sales

dim_store
   │
   └──────► fact_sales
```

This structure allows users to analyze sales and inventory metrics by:

* Date
* Year
* Month
* Week
* Store
* Region
* Product
* Category

---

# 9. Data Quality & Validation

Data preparation and validation included checks for:

* Data types
* Missing values
* Duplicate records
* Date consistency
* Numeric field validity
* Flag values
* Product and store identifiers
* Sales and inventory fields
* Forecast values

Additional validation was performed during the Power BI modeling and dashboard development process.

---

# 10. Important Dataset Considerations

### Weekly Grain

The dataset represents **weekly observations**, rather than individual customer transactions.

Therefore, metrics and trends should be interpreted at the weekly/store/product level.

### Forecast Interpretation

Forecast values represent expected demand and are evaluated against actual sales.

Forecast error does not necessarily indicate that the forecasting method itself is incorrect; differences can also arise from promotions, holidays, stockouts, pricing changes, and demand variability.

### Stockout Interpretation

A stockout flag indicates an observed stockout condition. It should not automatically be interpreted as the exact quantity of lost sales unless additional lost-sales data is available.

---

# 11. Privacy & Data Availability

The complete dataset is intentionally **not included in this public portfolio repository**.

This repository provides:

* Data structure
* Field definitions
* Business context
* Analytical methodology
* Dashboard screenshots
* Project documentation
* Business insights

This allows the project methodology and analytical approach to be demonstrated without exposing the complete underlying dataset.

---

## 📌 Summary

The dataset provides a structured foundation for analyzing the relationship between:

**Sales + Inventory + Promotions + Holidays + Pricing + Forecasting**

The resulting Power BI solution converts these data points into business KPIs, interactive visualizations, performance analysis, and actionable recommendations.
