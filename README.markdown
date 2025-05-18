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


📊 Task 2: Exploratory Data Analysis (EDA)
✅ Insights & Actions:
- Used `.describe()` for statistical summary
- Identified top 10 selling products by Quantity
- Calculated total revenue and number of transactions

📈 Task 3: Time Series Analysis
✅ Objective:
- Analyze total sales trends over time.
📌 Visual:
- 📈 Highest Sales: November 2011 (~1.46M)
- 📉 Lowest Sales: December 2011 (~433K)
![Image](https://github.com/user-attachments/assets/5c6daaaa-9ec7-4bd9-9de2-1f527cc17773)

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
![Image](https://github.com/user-attachments/assets/6ba9e0bd-8bbb-4314-9465-80bc6f209425)

🛒 Task 5: Product Category Analysis
✅ Actions:
- Extracted product categories from the Description column
- Aggregated sales and revenue per category
- Visualized the top 5 categories by revenue using bar plots
📌 Visual:
![Image](https://github.com/user-attachments/assets/31cebc4a-e65c-4aff-815a-e7f4a7708c4a)

🌍 Task 6: Geographical Analysis
✅ Actions:
- Calculated total revenue per country
- Visualized top 10 countries by revenue
- Computed percentage contribution of the top 3 countries (UK: 84.6%, EIRE: 10.1%, Netherlands: 2.7%)
📌 Visual:

![Image](https://github.com/user-attachments/assets/99b59e65-5cd3-4bcd-8342-1643d8db6d13)

🧑‍💼 Task 7: Customer Behavior Analysis
✅ Visualizations:
- Distribution of order quantities
- Scatter plot of Quantity vs. TotalPrice
- Weekly average sales behavior
  - 📌 Highest average sales on Thursday (~20)
  - 📌 Lowest average sales on Sunday (~12.5)
📌 Visual:

![Image](https://github.com/user-attachments/assets/10e91c28-dbb1-42f2-825c-a40ed5700326)

📉 Task 8: Moving Average Forecasting
✅ Implementation:
- Computed 7-day moving average
- Visualized actual sales vs. moving average (last 3 months)
📌 Visual:
![Image](https://github.com/user-attachments/assets/762349d0-9939-46c0-9089-b0cccedf48ef)

📊 Task 9: Summary Dashboard
✅ Dashboard Views:
- Created a 2x2 subplot showing:
  - Monthly sales trend
  - Top 5 products by revenue
  - Customer segment distribution
  - Top 5 countries by revenue (logarithmic scale)
📌 Visual:

![Image](https://github.com/user-attachments/assets/df8206e8-dd2b-4841-9a9d-4612531d8476)

⚙️ Task 10: Optimization of Data Processing
✅ Goal:
- Compare performance between:
  - Loop-based approach
  - Vectorized operations
📌 Result:
- Vectorized operations performed significantly faster and more efficiently.

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
