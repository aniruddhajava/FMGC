#  FMCG Sales Analysis using Python

##  Project Overview

This project analyzes **FMCG (Fast-Moving Consumer Goods) sales data** using Python and Pandas.

The objective is to clean and transform raw sales data, perform exploratory sales analysis, identify important business trends, and generate an Excel report containing the key analysis results.

The project demonstrates practical **Data Analyst skills including data cleaning, data transformation, aggregation and automated report generation**.

---

##  Technologies & Tools

* **Python**
* **Pandas** – Data cleaning and analysis
* **NumPy** – Numerical operations
* **Seaborn** – Data visualization
* **Jupyter Notebook**
* **Excel** – Final analysis report

---

##  Dataset

The dataset contains FMCG sales information with the following columns:

| Column         | Description                            |
| -------------- | -------------------------------------- |
| `Month`        | Sales month/date                       |
| `Channel`      | Sales channel                          |
| `Sub-Channels` | Specific sales platform or sub-channel |
| `Product Name` | Product name including packaging size  |
| `Category`     | Product category                       |
| `Qty`          | Quantity sold                          |
| `Sales`        | Sales amount                           |


##  Data Cleaning & Preprocessing

The raw dataset required several preprocessing steps before analysis.

### 1. Date Conversion

The `Month` column was converted from text into a proper datetime format:

```python
df["Month"] = pd.to_datetime(df["Month"], format="%d-%b-%y")
```

### 2. Sales Conversion

Sales values contained commas, so they were cleaned and converted into integers:

```python
df["Sales"] = df["Sales"].astype(str).str.replace(",", "").astype(int)
```

### 3. Quantity Conversion

The quantity column was also cleaned and converted into numeric format:

```python
df["Qty"] = df["Qty"].astype(str).str.replace(",", "").astype(int)
```

After preprocessing:

* `Month` → `datetime64`
* `Qty` → `int64`
* `Sales` → `int64`
* Other categorical columns → `object`

The resulting dataset contained **224 non-null records** across all seven columns.

---

##  Analysis Performed

### 1. Month-wise Quantity Sold

Monthly quantities were calculated using Pandas `groupby()` and aggregation.

```python
monthly_qty = df.groupby(["Month"])[["Qty"]].sum()
```

Results:

| Month    | Quantity Sold |
| -------- | ------------: |
| Dec 2020 |        85,427 |
| Jan 2021 |        67,732 |
| Feb 2021 |        76,393 |
| Mar 2021 |        80,229 |

The analysis shows that **December 2020 recorded the highest quantity sold** among the four months.

---

### 2. Category-wise Quantity Sold

The total quantity sold was calculated for each product category.

```python
category_qty = df.groupby(["Category"])[["Qty"]].sum()
```

| Category             | Quantity Sold |
| -------------------- | ------------: |
| Functional Nutrition |        42,354 |
| Gourmet Nutrition    |        89,279 |
| Juices               |       178,148 |

**Juices** recorded the highest quantity sold.

---

### 3. Sub-Channel-wise Sales

Sales were aggregated by individual sub-channel.

```python
sub_Tsales = df.groupby(["Sub-Channels"])[["Sales"]].sum()
```

Some of the major sales contributors were:

| Sub-Channel    |       Sales |
| -------------- | ----------: |
| AMAZON         | ₹35,606,804 |
| D2C            | ₹28,047,538 |
| Offline - MT   |  ₹6,257,948 |
| Offline - West |  ₹6,441,427 |
| Flipkart       |  ₹3,770,415 |

**AMAZON** generated the highest sales among the sub-channels in the analysis.

---

### 4. Highest Sales Month

Monthly sales were aggregated and sorted to identify the month with the highest sales.

```python
high_sales_month = (
    df.groupby(["Month"])[["Sales"]]
    .sum()
    .sort_values(by="Sales", ascending=False)
    .head(1)
)
```

**March 2021** recorded the highest monthly sales:

**₹25,127,827**

---

### 5. Highest-Selling Product

Packaging sizes were removed from product names so that different package sizes could be analyzed as the same product.

For example:

* `GET SLIM JUICE 1 L`
* `GET SLIM JUICE 500 ML`

were treated as the same product.

```python
df["Product"] = df["Product Name"].str.replace(
    r"\s\d+\s?(ML|L|KG|G)",
    "",
    regex=True
)
```

The highest-selling product based on sales was:

###  ALOE + GARCINIA JUICE

**Total Sales: ₹12,249,252**

---

### 6. Product Portfolio Analysis

The project also generated a standardized list of different products offered by the company.

```python
unique_product = pd.DataFrame(
    df["Product_Name"].unique(),
    columns=["Different Products"]
)
```

This helped identify unique products independently of their packaging quantities.

---

##  Excel Report

The analysis results were exported to an Excel workbook using Pandas `ExcelWriter`.

```python
with pd.ExcelWriter("Sales_Report.xlsx") as writer:

    monthly_qty.to_excel(writer, sheet_name="Monthly Qty")
    category_qty.to_excel(writer, sheet_name="Category Qty")
    sub_Tsales.to_excel(writer, sheet_name="Top Sales")
    high_sales_month.to_excel(writer, sheet_name="High Sales Month")
    top_product.to_excel(writer, sheet_name="Top Product")
    unique_product.to_excel(writer, sheet_name="All Product")
```

###  Report Sheets

The generated `Sales_Report.xlsx` contains:

1. **Monthly Qty**
2. **Category Qty**
3. **Top Sales**
4. **High Sales Month**
5. **Top Product**
6. **All Product**

The notebook explicitly creates these six analysis sheets.

---

## 📁 Project Structure

```text
FMCG-Sales-Analysis/
│
├── FMGC(1).ipynb
├── Sales_Report.xlsx
├── Raw Sales Data.csv
└── README.md
```

---

## 🚀 How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/FMCG-Sales-Analysis.git
```

### 2. Navigate to the project

```bash
cd FMCG-Sales-Analysis
```

### 3. Install required libraries

```bash
pip install pandas numpy seaborn openpyxl jupyter
```

### 4. Open the Jupyter Notebook

```bash
jupyter notebook
```

Open:

```text
FMGC(1).ipynb
```
---

⭐ If you found this project useful, consider giving the repository a **star**!

