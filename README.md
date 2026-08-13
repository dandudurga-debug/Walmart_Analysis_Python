Walmart Sales Analysis & Forecasting – Python-Only Project

1. Project Overview
This project performs an end-to-end analysis of historical Walmart weekly sales data using Python. The objective is to understand sales performance across stores, identify time-based and seasonal sales patterns, analyze holiday effects, evaluate relationships with external factors, and forecast future sales using the Prophet time-series forecasting model. This is a Python-only analytics and forecasting project; no Power BI or dashboard implementation is included.
2. Business Objective
•	Analyze historical Walmart weekly sales data.
•	Measure overall sales performance using business KPIs.
•	Compare sales performance across stores.
•	Identify monthly and time-based sales trends.
•	Compare holiday and non-holiday sales.
•	Analyze relationships between sales and external variables.
•	Identify trend and seasonal patterns.
•	Forecast future weekly sales for approximately 52 weeks.
•	Provide business insights and recommendations.
3. Business Questions
1.	What is the total sales generated?
2.	What is the average weekly sales?
3.	Which stores have the highest sales?
4.	Which stores have the lowest sales?
5.	How do sales change over time?
6.	What are the monthly sales trends?
7.	How do holiday and non-holiday sales compare?
8.	What relationships exist between sales and external variables?
9.	Is there a seasonal pattern in Walmart sales?
10.	What are the expected sales for the next 52 weeks?
4. Dataset
The project uses the Walmart historical sales dataset.
Column	Description
Store	Unique Walmart store identifier.
Date	Weekly sales date.
Weekly_Sales	Weekly sales amount.
Holiday_Flag	1 = Holiday week; 0 = Non-holiday week.
Temperature	Temperature value.
Fuel_Price	Fuel price.
CPI	Consumer Price Index.
Unemployment	Unemployment rate.

Dataset statistics from the supplied analysis:
•	45 stores
•	6,435 records after duplicate handling
•	8 columns
•	Total Sales: approximately $6.74 Billion
•	Average Weekly Sales: approximately $1.047 Million
5. Technologies & Libraries
•	Python
•	Pandas
•	NumPy
•	Matplotlib
•	Seaborn
•	Statsmodels
•	Prophet
6. Project Workflow
Walmart.csv
   ↓
Data Loading
   ↓
Data Quality Check
   ↓
Duplicate Removal
   ↓
Date Conversion
   ↓
Exploratory Data Analysis
   ↓
KPI Analysis
   ↓
Correlation Analysis
   ↓
Monthly Sales Trend
   ↓
Holiday Analysis
   ↓
Store-Level Analysis
   ↓
Time-Series Decomposition
   ↓
Prophet Forecasting
   ↓
Business Insights & Recommendations
7. Data Loading & Preprocessing
The dataset is loaded using Pandas.
df = pd.read_csv('Walmart.csv')
Missing values are checked using:
missing_values = df.isnull().sum()
Duplicate records are removed using:
df.drop_duplicates(inplace=True)
The Date column is converted to datetime:
df['Date'] = pd.to_datetime(df['Date'], format='%d-%m-%Y')
8. Exploratory Data Analysis
•	Histogram of Weekly_Sales to understand the sales distribution.
•	Scatter plot of Store versus Weekly_Sales to compare store performance.
•	Descriptive statistics using df.describe().
9. Business KPI Analysis
KPI	Calculation	Current Result
Total Sales	Sum of Weekly_Sales	6,737,218,987.11
Sales Records	Count of Weekly_Sales	6,435
Average Weekly Sales	Mean of Weekly_Sales	1,046,964.88 approx.
10. Correlation Analysis
A correlation matrix is calculated for numerical variables and visualized using a Seaborn heatmap. The analysis helps identify associations between Weekly_Sales and variables such as Temperature, Fuel_Price, CPI, Unemployment, Store, and Holiday_Flag. Correlation indicates association and does not establish causation.
correlation_matrix = df.corr()
11. Monthly Sales Trend
Weekly sales are aggregated into monthly sales to identify sales peaks, declines, and seasonal fluctuations.
monthly_sales = (
    df.groupby(pd.Grouper(key='Date', freq='ME'))['Weekly_Sales']
      .sum()
      .reset_index()
)
12. Holiday Analysis
Sales are grouped using Holiday_Flag to compare holiday and non-holiday sales.
holiday_sales = df.groupby('Holiday_Flag')['Weekly_Sales'].sum()
•	Non-Holiday Sales: approximately $6.232 Billion.
•	Holiday Sales: approximately $505.3 Million.
•	Interpretation should consider the number of holiday and non-holiday observations; total sales alone does not establish holiday impact.
13. Store-Level Analysis
Total sales are calculated for each store to identify high- and low-performing stores.
sales_by_store = df.groupby('Store')['Weekly_Sales'].sum()
14. Time-Series Analysis
Sales are aggregated by date and used for time-series decomposition.
sales_by_date = df.groupby('Date')['Weekly_Sales'].sum()
15. Time-Series Decomposition
Statsmodels is used to decompose the weekly sales series into observed, trend, seasonal, and residual components. A 52-week period is used to represent approximately annual seasonality.
from statsmodels.tsa.seasonal import seasonal_decompose

decomposition = seasonal_decompose(
    sales_by_date,
    model='additive',
    period=52
)
16. Sales Forecasting using Prophet
Prophet is used to forecast approximately 52 future weeks.
from prophet import Prophet

prophet_df = pd.DataFrame({
    'ds': df['Date'],
    'y': df['Weekly_Sales']
})

model = Prophet()
model.fit(prophet_df)

future = model.make_future_dataframe(
    periods=52,
    freq='W'
)

forecast = model.predict(future)
17. Forecast Visualization
fig1 = model.plot(forecast)
fig2 = model.plot_components(forecast)
The forecast visualization displays historical observations, predicted sales, and uncertainty intervals. Forecast component plots show the model's trend and seasonal components.
18. Key Analytical Insights
•	Total historical sales are approximately $6.74 billion.
•	Average weekly sales are approximately $1.047 million.
•	Sales vary considerably across stores.
•	Monthly and weekly analysis helps identify sales peaks and declines.
•	Holiday and non-holiday sales can be compared using Holiday_Flag.
•	Time-series decomposition helps separate trend and seasonality.
•	Prophet provides a baseline 52-week forecast for future planning.
19. Business Recommendations
11.	Investigate high-performing stores to identify successful operational practices.
12.	Investigate low-performing stores for potential business or operational issues.
13.	Use historical seasonal patterns for future sales planning.
14.	Monitor holiday periods separately.
15.	Track economic variables alongside sales performance.
16.	Compare future actual sales with forecast values to assess forecasting performance.
17.	Extend the analysis with inventory, product, promotion, and customer data when available.
20. Project Limitations
•	The dataset is historical and not real-time.
•	Customer-level information is unavailable.
•	Product-level information is unavailable.
•	Inventory and promotion information is unavailable.
•	Correlation does not establish causation.
•	Forecast accuracy depends on historical patterns and data quality.
•	Unexpected future events may affect forecast accuracy.
21. Future Enhancements
•	Add automated data-quality validation.
•	Create reusable Python functions.
•	Implement time-series train/test validation.
•	Add MAE, RMSE, and MAPE forecasting metrics.
•	Compare Prophet with ARIMA/SARIMA and machine-learning models.
•	Add sales anomaly detection.
•	Automate PDF or Excel reporting.
•	Integrate product, inventory, promotion, and customer data.
22. Recommended GitHub Structure
Walmart-Sales-Analysis/
├── README.md
├── BRD/
│   └── Walmart_Sales_Analysis_BRD.docx
├── FRD/
│   └── Walmart_Sales_Analysis_FRD.docx
├── Python/
│   └── Walmart_Sales_Analysis.py
├── Data/
│   └── Walmart.csv
├── Outputs/
│   ├── sales_distribution.png
│   ├── sales_vs_store.png
│   ├── correlation_heatmap.png
│   ├── monthly_sales_trend.png
│   ├── time_series_decomposition.png
│   ├── forecast.png
│   └── forecast_components.png
└── requirements.txt
23. Installation
Install the required Python libraries:
pip install pandas numpy matplotlib seaborn statsmodels prophet
Or install dependencies from requirements.txt:
pip install -r requirements.txt
24. How to Run
Place Walmart.csv in the Data folder and execute the Python analysis script.
python Python/Walmart_Sales_Analysis.py

25. Documentation
•	BRD – Business Requirements Document
•	FRD – Functional Requirements Document
•	README – Project overview and execution guide
•	Python Analysis Script
•	Walmart Dataset
•	Analytical Output Charts

26. Author
Durga Venkateswarababu Dandu
Data Analyst | Python | SQL | Power BI | Excel | Data Analytics | Forecasting
