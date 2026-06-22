# 📊 Sales Dashboard & Trend Analyzer

An industry-level data analytics project built using Python to clean, analyze, and visualize corporate sales data. This project processes 133 transactions across multiple product categories and regions over a 12-month period to deliver actionable business intelligence.

---

## 🔧 Core Technologies Used
* **Python 3** (Core Processing)
* **NumPy** (Vectorized Statistical Calculations & Normalization)
* **Pandas** (Data Manipulation, Missing Value Treatment, GroupBy Aggregations, & DataFrame Merging)
* **Matplotlib & Seaborn** (Data Visualization & Dashboard Design)

---

## 📂 Project Structure & Datasets
* `Sales_Dashboard_Trend_Analyzer.ipynb`: The main Jupyter Notebook containing the full execution pipeline.
* `sales.csv`: The primary sales dataset containing transactional data with missing records for data cleaning practice.
* `product_info.csv`: Auxiliary product database used for executing relational inner joins (`pd.merge`).

### 📊 Primary Dataset Schema (`sales.csv`)
| Column Name | Data Type | Description | Example |
| :--- | :--- | :--- | :--- |
| **Sales_Date** | Object ➡️ Datetime | Date of the transaction | `2024-01-03` |
| **Product** | Object (String) | Name of the item sold | `Laptop`, `Mobile Phone` |
| **Category** | Object (String) | Department classification | `Electronics`, `Furniture` |
| **Region** | Object (String) | Geographic sales territory | `North`, `South`, `East`, `West` |
| **Quantity** | Float64 | Number of units sold (with NaNs) | `5.0`, `12.0` |
| **Price_Per_Unit**| Float64 | Selling price per single item | `₹55000.00` |
| **Total_Revenue** | Float64 | Total financial intake (with NaNs) | `₹275000.00` |
| **Cost_Price** | Int64 ➡️ Float64 | Manufacturing/Wholesale cost | `₹42000.00` |
| **Agent_Name** | Object (String) | Salesperson responsible | `Rahul`, `Priya` |

---

## 🚀 Key Implementation Steps

### 1. Advanced Data Preprocessing & Cleaning
* **Missing Value Imputation**: Handled structural missing values using average distribution parameters via `.fillna()`.
* **Type Casting**: Optimized memory mapping by converting integers/strings into `np.float64` and parsing text-based dates into structural `datetime64` objects.
* **Feature Engineering**: Built automated lambda pipelines via `.apply()` to dynamically generate `Month`, `Quarter`, and `Profit_Margin` columns:
  $$Profit\_Margin = \frac{Total\_Revenue - (Quantity \times Cost\_Price)}{Total\_Revenue} \times 100$$

### 2. Mathematical Modeling & Statistical Analysis
* Leveraged **NumPy** engine to run high-performance computations for business KPIs:
  * `np.mean()` (Average Transaction Value)
  * `np.std()` (Revenue Dispersion/Risk Assessment)
  * `np.var()` (Market Volatility Analysis)

### 3. Relational Data Merging & Advanced GroupBy
* Performed multidimensional data aggregation using `.groupby(['Product', 'Region'])` paired with `.agg(['sum', 'mean', 'count'])` to assess regional performance.
* Combined disparate transactional tables and metadata information using `pd.merge()` with an explicit `inner` join.

---

## 📊 Visual Insights & Dashboards Generated

The project compiles a multi-axes visual suite designed using a custom dark theme profile:

```text
1. 📊 Bar Chart     --> Category-wise Total Revenue Allocation
2. 📈 Line Plot     --> Monthly Sequential Sales Trend with Shaded Area Fills
3. 🍕 Pie Chart     --> Regional Revenue Share (% Contribution with Slice Explosion)
4. 📉 Scatter Plot  --> Quantity Sold vs. Total Revenue Correlation + Trend Line
5. 🪵 Histogram     --> Transactional Revenue Density Distribution
6. 🔥 Heatmap       --> Feature Correlation Matrix (Via Seaborn)
