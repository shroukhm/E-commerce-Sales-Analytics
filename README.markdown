📊 E-Commerce Sales Analytics Capstone Project
This project is a comprehensive analysis of an e-commerce sales dataset. The aim is to extract meaningful business insights through data cleaning, exploratory data analysis, customer segmentation, and time series forecasting. It serves as a full pipeline from raw data to actionable insights and dashboards.

🧠 Project Objectives
- Perform data preprocessing and cleaning.
- Conduct exploratory data analysis (EDA).
- Analyze customer behavior and segment them using RFM analysis.
- Visualize geographical and product-based performance.
- Implement forecasting with moving averages.
- Build a summary dashboard to display business insights.

📁 Project Structure
- `Capstone_Project_E_commerce_Analytics_Module.ipynb` — Jupyter Notebook with all code steps.
- `images/` — Folder containing saved visualization outputs.
- `README.md` — Project documentation (you’re reading it!).

🧹 Task 1: Data Cleaning
✅ Steps:
- Handled missing values in CustomerID
- Converted InvoiceDate to datetime format
- Removed rows with negative Quantity or UnitPrice
- Created a new column `TotalPrice = Quantity * UnitPrice`

```python
import pandas as pd

# Load the dataset
file_path = '/content/drive/MyDrive/data.csv'
try:
    data = pd.read_csv(file_path, encoding='ISO-8859-1')
except FileNotFoundError:
    print("Data Not Found !!!!")

# Handle missing values in CustomerID
data_cleaned = data.copy()
data_cleaned['CustomerID'] = data_cleaned['CustomerID'].astype(str).fillna('Unknown')

# Convert InvoiceDate to datetime type
if 'InvoiceDate' in data_cleaned.columns:
    data_cleaned['InvoiceDate'] = pd.to_datetime(data_cleaned['InvoiceDate'], errors='coerce')

# Remove rows with negative Quantity or UnitPrice
if 'Quantity' in data_cleaned.columns and 'UnitPrice' in data_cleaned.columns:
    data_cleaned = data_cleaned[(data_cleaned['Quantity'] >= 0) & (data_cleaned['UnitPrice'] >= 0)]

# Create a TotalPrice column
if 'Quantity' in data_cleaned.columns and 'UnitPrice' in data_cleaned.columns:
    data_cleaned['TotalPrice'] = data_cleaned['Quantity'] * data_cleaned['UnitPrice']

# Save the cleaned data
output_path = 'cleaned_data.csv'
data_cleaned.to_csv(output_path, index=False)

data_cleaned.head()
```

📊 Task 2: Exploratory Data Analysis (EDA)
✅ Insights & Actions:
- Used `.describe()` for statistical summary
- Identified top 10 selling products by Quantity
- Calculated total revenue and number of transactions

```python
# Statistical summary
print(data_cleaned.describe())

# Top 10 selling products by Quantity
top_products = data_cleaned.groupby('Description')['Quantity'].sum().sort_values(ascending=False).head(10)
print("Top 10 Products by Quantity Sold:")
print(top_products)

# Total revenue and number of transactions
total_revenue = data_cleaned['TotalPrice'].sum()
num_transactions = data_cleaned['InvoiceNo'].nunique()
print(f"Total Revenue: {total_revenue}")
print(f"Number of Transactions: {num_transactions}")
```

📈 Task 3: Time Series Analysis
✅ Objective:
- Analyze total sales trends over time.
📌 Visual:
- 📈 Highest Sales: November 2011 (~1.46M)
- 📉 Lowest Sales: December 2011 (~433K)

```python
import matplotlib.pyplot as plt

# Group by month and calculate total sales
data_cleaned['Month'] = data_cleaned['InvoiceDate'].dt.to_period('M')
monthly_sales = data_cleaned.groupby('Month')['TotalPrice'].sum()

# Plot monthly sales trend
plt.figure(figsize=(10, 6))
monthly_sales.plot(marker='o')
plt.title('Total Sales Over Months')
plt.xlabel('Month')
plt.ylabel('Total Sales')
plt.grid(True)
plt.savefig('images/monthly_sales_trend.png')
```

🧮 Task 4: RFM Customer Segmentation
✅ Metrics:
- Recency: Days since last purchase
- Frequency: Total transactions
- Monetary: Total amount spent
- Customers were segmented into:
  - High (17.2%)
  - Medium (34.2%)
  - Low (48.6%)
📌 Visual:

```python
import pandas as pd
from datetime import datetime

# Define reference date for recency calculation
reference_date = datetime(2011, 12, 31)

# Calculate RFM metrics
rfm = data_cleaned.groupby('CustomerID').agg({
    'InvoiceDate': lambda x: (reference_date - x.max()).days,  # Recency
    'InvoiceNo': 'nunique',  # Frequency
    'TotalPrice': 'sum'  # Monetary
}).rename(columns={'InvoiceDate': 'Recency', 'InvoiceNo': 'Frequency', 'TotalPrice': 'Monetary'})

# Segment customers using quantiles
rfm['R_Score'] = pd.qcut(rfm['Recency'], 3, labels=[3, 2, 1])
rfm['F_Score'] = pd.qcut(rfm['Frequency'].rank(method='first'), 3, labels=[1, 2, 3])
rfm['M_Score'] = pd.qcut(rfm['Monetary'], 3, labels=[1, 2, 3])

# Combine scores to segment customers
rfm['RFM_Score'] = rfm['R_Score'].astype(int) + rfm['F_Score'].astype(int) + rfm['M_Score'].astype(int)
rfm['Segment'] = pd.cut(rfm['RFM_Score'], bins=[0, 5, 7, 9], labels=['Low', 'Medium', 'High'])

# Visualize segment distribution
segment_dist = rfm['Segment'].value_counts(normalize=True) * 100
plt.figure(figsize=(8, 6))
segment_dist.plot(kind='pie', autopct='%1.1f%%', title='Customer Segment Distribution')
plt.savefig('images/customer_segment_distribution.png')
```

🛒 Task 5: Product Category Analysis
✅ Actions:
- Extracted product categories from the Description column
- Aggregated sales and revenue per category
- Visualized the top 5 categories by revenue using bar plots
📌 Visual:

```python
# Extract product categories (simplified by taking the first word of Description)
data_cleaned['Category'] = data_cleaned['Description'].str.split().str[0]

# Aggregate sales and revenue per category
category_stats = data_cleaned.groupby('Category').agg({'TotalPrice': 'sum', 'Quantity': 'sum'}).reset_index()

# Top 5 categories by revenue
top_5_categories = category_stats.sort_values('TotalPrice', ascending=False).head(5)

# Visualize top 5 categories by revenue and sales
fig, ax1 = plt.subplots(figsize=(10, 6))
ax1.bar(top_5_categories['Category'], top_5_categories['TotalPrice'], color='skyblue', label='Revenue')
ax2 = ax1.twinx()
ax2.bar(top_5_categories['Category'], top_5_categories['Quantity'], color='lightcoral', alpha=0.5, label='Sales')
ax1.set_title('Top 5 Product Categories by Revenue and Sales')
ax1.set_ylabel('Revenue')
ax2.set_ylabel('Sales')
ax1.legend(loc='upper left')
ax2.legend(loc='upper right')
plt.savefig('images/top_5_categories.png')
```

🌍 Task 6: Geographical Analysis
✅ Actions:
- Calculated total revenue per country
- Visualized top 10 countries by revenue
- Computed percentage contribution of the top 3 countries (UK: 84.6%, EIRE: 10.1%, Netherlands: 2.7%)
📌 Visual:

```python
# Calculate total revenue per country
country_revenue = data_cleaned.groupby('Country')['TotalPrice'].sum().sort_values(ascending=False)

# Top 10 countries by revenue
top_10_countries = country_revenue.head(10)

# Visualize top 10 countries (linear and logarithmic scale)
plt.figure(figsize=(10, 6))
top_10_countries.plot(kind='bar', title='Top 10 Countries by Revenue')
plt.ylabel('Total Revenue')
plt.savefig('images/top_10_countries_revenue.png')

plt.figure(figsize=(10, 6))
top_10_countries.plot(kind='bar', logy=True, title='Top 10 Countries by Revenue (Logarithmic Scale)')
plt.ylabel('Total Revenue (Log)')
plt.savefig('images/top_10_countries_revenue_log.png')

# Percentage contribution of top 3 countries
top_3_countries = country_revenue.head(3)
top_3_percentage = (top_3_countries / country_revenue.sum()) * 100
plt.figure(figsize=(8, 6))
top_3_percentage.plot(kind='pie', autopct='%1.1f%%', title='Percentage of Total Revenue from Top 3 Countries')
plt.savefig('images/top_3_countries_percentage.png')
```

🧑‍💼 Task 7: Customer Behavior Analysis
✅ Visualizations:
- Distribution of order quantities
- Scatter plot of Quantity vs. TotalPrice
- Weekly average sales behavior
  - 📌 Highest average sales on Thursday (~20)
  - 📌 Lowest average sales on Sunday (~12.5)
📌 Visual:

```python
# Distribution of order quantities
plt.figure(figsize=(10, 6))
data_cleaned['Quantity'].hist(bins=50, range=(0, 20))
plt.title('Distribution of Order Quantities (Without Outliers)')
plt.xlabel('Order Quantity')
plt.ylabel('Frequency')
plt.savefig('images/order_quantity_distribution.png')

# Scatter plot of Quantity vs TotalPrice
plt.figure(figsize=(10, 6))
plt.scatter(data_cleaned['Quantity'], data_cleaned['TotalPrice'], alpha=0.5)
plt.title('Quantity vs. TotalPrice')
plt.xlabel('Quantity')
plt.ylabel('TotalPrice')
plt.savefig('images/quantity_vs_totalprice.png')

# Weekly average sales behavior
data_cleaned['DayOfWeek'] = data_cleaned['InvoiceDate'].dt.day_name()
weekly_sales = data_cleaned.groupby('DayOfWeek')['TotalPrice'].mean().reindex(
    ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
)
plt.figure(figsize=(10, 6))
weekly_sales.plot(kind='bar', title='Average Daily Sales Throughout the Week')
plt.ylabel('Average Sales')
plt.savefig('images/weekly_sales_behavior.png')
```

📉 Task 8: Moving Average Forecasting
✅ Implementation:
- Computed 7-day moving average
- Visualized actual sales vs. moving average (last 3 months)
📌 Visual:

```python
# Daily sales for the last 3 months
last_3_months = data_cleaned[data_cleaned['InvoiceDate'] >= '2011-09-01']
daily_sales = last_3_months.groupby(last_3_months['InvoiceDate'].dt.date)['TotalPrice'].sum()

# Compute 7-day moving average
moving_avg = daily_sales.rolling(window=7).mean()

# Visualize actual sales vs moving average
plt.figure(figsize=(12, 6))
daily_sales.plot(label='Actual Sales', color='blue')
moving_avg.plot(label='7-Day Moving Average', color='orange')
plt.title('Daily Sales vs. 7-Day Moving Average (Last 3 Months)')
plt.xlabel('Date')
plt.ylabel('Sales (TotalPrice)')
plt.legend()
plt.savefig('images/sales_vs_moving_average.png')
```

📊 Task 9: Summary Dashboard
✅ Dashboard Views:
- Created a 2x2 subplot showing:
  - Monthly sales trend
  - Top 5 products by revenue
  - Customer segment distribution
  - Top 5 countries by revenue (logarithmic scale)
📌 Visual:

```python
# Create a 2x2 subplot dashboard
fig, axes = plt.subplots(2, 2, figsize=(15, 10))

# Monthly sales trend
monthly_sales.plot(ax=axes[0, 0], marker='o', title='Monthly Sales Trend')
axes[0, 0].set_ylabel('Total Sales')

# Top 5 products by revenue
top_5_products = data_cleaned.groupby('Description')['TotalPrice'].sum().sort_values(ascending=False).head(5)
top_5_products.plot(kind='bar', ax=axes[0, 1], title='Top 5 Products by Revenue')
axes[0, 1].set_ylabel('Total Revenue')

# Customer segment distribution
segment_dist.plot(kind='pie', autopct='%1.1f%%', ax=axes[1, 0], title='Customer Segment Distribution')

# Top 5 countries by revenue (logarithmic scale)
top_5_countries = country_revenue.head(5)
top_5_countries.plot(kind='bar', logy=True, ax=axes[1, 1], title='Top 5 Countries by Revenue (Logarithmic Scale)')
axes[1, 1].set_ylabel('Total Revenue (Log)')

plt.tight_layout()
plt.savefig('images/summary_dashboard.png')
```

⚙️ Task 10: Optimization of Data Processing
✅ Goal:
- Compare performance between:
  - Loop-based approach
  - Vectorized operations
📌 Result:
- Vectorized operations performed significantly faster and more efficiently.

```python
import time

# Loop-based approach to calculate TotalPrice
start_time = time.time()
total_price_loop = []
for i in range(len(data_cleaned)):
    total_price_loop.append(data_cleaned.iloc[i]['Quantity'] * data_cleaned.iloc[i]['UnitPrice'])
loop_time = time.time() - start_time
print(f"Loop-based approach time: {loop_time} seconds")

# Vectorized approach
start_time = time.time()
total_price_vectorized = data_cleaned['Quantity'] * data_cleaned['UnitPrice']
vectorized_time = time.time() - start_time
print(f"Vectorized approach time: {vectorized_time} seconds")

# Compare performance
print(f"Vectorized is {loop_time / vectorized_time:.2f} times faster than loop-based.")
```

🛠 Tools & Technologies
- Language: Python
- Libraries: pandas, matplotlib, seaborn, numpy, datetime
- IDE: Jupyter Notebook

📌 Conclusion
This project demonstrates the end-to-end process of e-commerce sales analysis. Key insights include:
- Sales peak in November 2011 and dip in December 2011, with Thursdays showing the highest average daily sales.
- Customer segments show a majority in the Low category (48.6%), with targeted strategies needed for High (17.2%) and Medium (34.2%) segments.
- The United Kingdom dominates revenue (84.6%), with "WHITE HANGING HEART T-LIGHT HOLDER" leading product revenue.
- Order quantities are skewed towards lower values, and the 7-day moving average aids in forecasting trends.
The visualizations in the `images/` folder support these findings, offering a clear dashboard for decision-making.