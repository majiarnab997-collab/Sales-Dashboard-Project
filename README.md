# 📊 Sales Dashboard & Trend Analyzer

A Python-based data analysis project that loads sales data, cleans it, derives insights using NumPy, and visualizes trends with Matplotlib.

---

## 🗂️ Project Structure

```
Sales-Dashboard/
├── Sales_Dashboard_Trend_Analyzer.ipynb   # Main notebook
├── sales.csv                              # Raw sales data
├── Product info.csv                       # Product metadata
└── README.md
```

---

## ⚙️ Tech Stack

| Library       | Purpose                                      |
|---------------|----------------------------------------------|
| `pandas`      | Data loading, cleaning, and transformation   |
| `numpy`       | Statistical analysis (mean, std, variance)   |
| `matplotlib`  | Data visualization (bar, line, pie, histogram)|

---

## 📋 Dataset Overview

The `sales.csv` file contains transactional sales records with the following columns:

| Column          | Type       | Description                            |
|-----------------|------------|----------------------------------------|
| `Date`          | datetime   | Date of the transaction                |
| `Product`       | string     | Product name                           |
| `Category`      | string     | Product category                       |
| `Region`        | string     | Sales region (North/South/East/West)   |
| `Salesperson`   | string     | Name of the agent                      |
| `Units_Sold`    | float      | Number of units sold                   |
| `Unit_Price`    | float      | Price per unit (₹)                     |
| `Total_Revenue` | float      | Total revenue for the transaction (₹)  |
| `Cost_Price`    | float      | Cost price per unit (₹)               |

> After preprocessing, three new columns are added: `Month`, `Quarter`, and `Profit_Margin`.

---

## 🔧 Steps & Questions Solved

### Q1 — Data Cleaning (Pandas)

**A. Handle Missing Values**  
Missing values in `Units_Sold`, `Unit_Price`, `Total_Revenue`, and `Cost_Price` are filled with the column mean using `fillna()`.

**B. Rename Columns**  
Columns are renamed for clarity:

| Old Name      | New Name         |
|---------------|------------------|
| `Date`        | `Sales_Date`     |
| `Units_Sold`  | `Quantity`       |
| `Unit_Price`  | `Price_Per_Unit` |
| `Salesperson` | `Agent_Name`     |

**C. Change Data Types**  
- `Cost_Price` → converted to `float64` using `astype(np.float64)`
- `Sales_Date` → converted to `datetime` using `pd.to_datetime()`

---

### Q2 — Statistical Analysis (NumPy)

Three statistical metrics computed on `Total_Revenue`:

| Metric              | Function      | Description                          |
|---------------------|---------------|--------------------------------------|
| Mean                | `np.mean()`   | Average revenue per transaction      |
| Standard Deviation  | `np.std()`    | Spread of revenue values             |
| Variance            | `np.var()`    | Squared standard deviation           |

---

### Q3 — Feature Engineering (apply + lambda)

New columns added using `.apply()` with `lambda` functions:

| New Column      | Source Column  | Logic                                           |
|-----------------|----------------|-------------------------------------------------|
| `Month`         | `Sales_Date`   | `lambda x: x.month` → extracts month number    |
| `Quarter`       | `Sales_Date`   | `lambda x: x.quarter` → extracts quarter (1–4) |
| `Profit_Margin` | Row-wise       | `((Revenue - Cost×Qty) / Revenue) × 100`       |

---

### Q4 — Visualizations (Matplotlib)

Four charts generated to explore the data visually:

#### 4.1 — Bar Chart: Total Revenue by Category

Groups data by `Category`, sums `Total_Revenue`, and plots a dark-themed bar chart with value annotations.

```python
grouped=sales_data.groupby("Category")["Total_Revenue"].sum().reset_index()
x_axis=grouped["Category"]
y_axis=grouped["Total_Revenue"]/100_00_00

fig,ax=plt.subplots(figsize=(10,5))
fig.patch.set_facecolor("#181824")
ax.set_facecolor("#181824")
ax.bar(x_axis,y_axis,color=["#00D2FF", "#FF6B6B", "#FFD24C", "#6BCB77"],edgecolor="black")
ax.set_title("Category-wise Total Revenue",pad=20,color="white")
ax.set_xlabel("Category",labelpad=12,color="darkgray")
ax.set_ylabel("Total Revenue (in Millions)",labelpad=12,color="darkgray")
plt.xticks(rotation=45,color="darkgray",fontsize=11)
plt.yticks(color="darkgray",fontsize=11)
for x, y in zip(x_axis, y_axis):
    ax.annotate(
        f"₹{y:.1f}M",
        xy=(x, y),
        xytext=(0, 10),
        textcoords="offset points",
        ha="center",
        va="bottom",
        fontsize=10,
        color="white",
        weight="bold",
    )
    for spine in ["top", "right"]:
       ax.spines[spine].set_visible(False)
    for spine in ["left","bottom"]:
        ax.spines[spine].set_color("#333345")

plt.show()
```
<img width="1072" height="720" alt="Screenshot 2026-06-22 100140" src="https://github.com/user-attachments/assets/b1374a9f-dc7f-4c9d-a295-8f50a268030e" />

#### 4.2 — Line Plot: Monthly Sales Trend (2024)

Groups data by `Month`, maps numbers to month names (Jan–Dec), and draws a filled-area line chart showing revenue movement across the year.

```python
month_map = {
    1: "Jan",
    2: "Feb",
    3: "Mar",
    4: "Apr",
    5: "May",
    6: "Jun",
    7: "Jul",
    8: "Aug",
    9: "Sep",
    10: "Oct",
    11: "Nov",
    12: "Dec",
}
grouped_sales["Month_Name"] = grouped_sales["Month"].map(month_map)

x_axis=grouped_sales["Month_Name"]
y_axis=grouped_sales["Total_Revenue"]/100_00_00

fig,ax=plt.subplots(figsize=(11,6))
fig.patch.set_facecolor("#181824")
ax.set_facecolor("#181824")
ax.plot(x_axis,y_axis,color="dodgerblue",marker="o",linewidth=2,markersize=11,markerfacecolor="#FFD24C",markeredgecolor="white",markeredgewidth=2.5,zorder=3,)
ax.fill_between(x_axis,y_axis,color="#00D2FF",alpha=0.15, zorder=2)
ax.set_title("Monthly Sales Trend (2024)", color="white", fontsize=15, weight="bold", pad=20)
ax.set_ylabel("Revenue (₹ Million)", color="darkgray", fontsize=12, labelpad=10)
ax.set_xlabel("Month", color="darkgray", fontsize=12, labelpad=10)


for x, y in zip(x_axis, y_axis):
    ax.annotate(
        f"₹{y:.1f}M",
        xy=(x, y),
        xytext=(0, 10),
        textcoords="offset points",
        ha="center",
        va="bottom",
        fontsize=10,
        color="white",
        weight="bold",
    )
    for spine in ["top", "right"]:
       ax.spines[spine].set_visible(False)
    for spine in ["left", "bottom"]:
       ax.spines[spine].set_color("#333345")
    plt.xticks(rotation=45,color="darkgray",fontsize=11)
    plt.yticks(color="darkgray",fontsize=11)
    ax.set_ylim(0, max(y_axis) + 1.5)
    plt.tight_layout()

plt.show()
```
<img width="1377" height="740" alt="Screenshot 2026-06-22 100153" src="https://github.com/user-attachments/assets/c904bb3a-a2e2-4c82-a88c-8f12770d7fd3" />

#### 4.3 — Pie Chart: Region-wise Revenue Share

Groups by `Region` and plots a pie chart with exploded slices showing percentage contribution of each region (North / South / East / West).

```python
x_axis=grouped_Region["Region"]
y_axis=grouped_Region["Total_Revenue"]

color_cullaction=["#00D2FF", "#FF6B6B", "#FFD24C", "#6BCB77"]
explodes=[0.05,0.05,0.05,0.05]

plt.pie(y_axis,labels=x_axis,colors=color_cullaction,explode=explodes,autopct="%1.1f%%",shadow=True)
plt.title("Region-wise Revenue Share",fontsize=20,weight="bold")
```
<img width="568" height="526" alt="Screenshot 2026-06-22 100202" src="https://github.com/user-attachments/assets/a8948335-7bfa-4d6f-9f36-0f31d35837c1" />

#### 4.4 — Histogram: Revenue Distribution

Plots a histogram of `Total_Revenue` (in thousands ₹) with 30 bins to show how transaction values are distributed.

```python
y_axis=sales_data["Total_Revenue"]/1000
fig,ax=plt.subplots(figsize=(10,6))
ax.hist(y_axis,bins=30,edgecolor="Black",color="dodgerblue")
fig.patch.set_facecolor("#181824")
ax.set_facecolor("#181824")
ax.set_xlabel("Transaction Revenue Range (in Thousands ₹)",labelpad=20,color="darkgray")
ax.set_ylabel("Frequency (Number of Transactions)",labelpad=20,color="darkgray")
ax.set_title("Revenue Distribution Across All Transactions",pad=20,color="White")
```
<img width="1362" height="743" alt="Screenshot 2026-06-22 100213" src="https://github.com/user-attachments/assets/195171fd-fb33-49a5-8657-2caa63cfb455" />

---

### Q5 — Multi-column GroupBy with Aggregation

Groups data by both `Product` and `Region`, then applies three aggregations simultaneously:

```python
sales_data.groupby(["Product", "Region"])["Total_Revenue"].agg(["sum", "mean", "count"]).reset_index()
```

| Aggregation | Meaning                              |
|-------------|--------------------------------------|
| `sum`       | Total revenue per Product+Region     |
| `mean`      | Average revenue per Product+Region   |
| `count`     | Number of transactions               |

---

### Q6 — DataFrame Merge (inner join)

Merges `sales.csv` with `Product info.csv` on the shared `Product` column using an inner join, combining product metadata with sales records.

```python
pd.merge(product_sales, sales, on="Product", how="inner")
```

---

## 🚀 How to Run

1. Clone this repository
2. Place `sales.csv` and `Product info.csv` in the same directory as the notebook
3. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib
   ```
4. Open and run the notebook:
   ```bash
   jupyter notebook Sales_Dashboard_Trend_Analyzer.ipynb
   ```

---

## 📌 Key Insights

- Revenue is broken down by **Category**, **Region**, and **Month** to identify top performers
- **Profit Margin** is computed per transaction to evaluate profitability
- **Monthly trend** reveals seasonal patterns across the year
- **Region-wise pie chart** shows which geography contributes most to total revenue
