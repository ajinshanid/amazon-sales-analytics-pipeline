# Amazon Sales Data Pipeline & Dashboard
### *End-to-End Cloud Analytics Project using Python, Snowflake, and Power BI*

---

## Overview
This project demonstrates a complete **data pipeline and analytics workflow** for Amazon Sales data from raw CSV ingestion to business insights.  The goal is to transform messy transactional data into a structured **star schema** in **Snowflake**, then visualize trends and through an interactive **Power BI dashboard**.

---

## Project Workflow

###  Data Cleaning (Python)
- Cleaned and formatted the dataset in **Pandas** to handle:
  - Missing or duplicate values  
  - Inconsistent date formats  
  - Category and region standardization  
- Exported the cleaned CSV for staging in Snowflake.

---

### Data Modeling in Snowflake
- Created a **Snowflake database and schema** (`AMAZON_SALES_DB.AMAZON_SALES_ANALYTICS`).  
- Designed a **Star Schema** with one fact table and five dimensions:
  - **DIM_PRODUCT** – Item Type  
  - **DIM_LOCATION** – Province & City  
  - **DIM_MANAGER** – Manager Details  
  - **DIM_CARRIER** – Shipment Carrier  
  - **DIM_DATE** – Sales & Ship Dates  
  - **FACT_SALES** – Core metrics such as Units Sold, Unit Price, Revenue, Cost, and Profit.  
- Used **MD5 hashing** to generate consistent surrogate keys for dimension joins.

`- Used MD5 hashing to generate consistent surrogate keys for dimension joins.

```sql
CREATE OR REPLACE TABLE FACT_SALES AS
SELECT
    SALES_ID AS SALE_ID,
    MD5(DATE::STRING) AS DATE_ID,
    MD5(ITEM_TYPE) AS PRODUCT_ID,
    MD5(PROVINCE || CITY) AS LOCATION_ID,
    MD5(MANAGER_EMAIL) AS MANAGER_ID,
    MD5(SHIPMENT_CARRIER) AS CARRIER_ID,
    UNITS_SOLD,
    UNIT_PRICE,
    UNIT_COST,
    TOTAL_REVENUE,
    TOTAL_COST,
    TOTAL_PROFIT
FROM STAGING_AMAZON_SALES;
```
## Data Report and Insights

This section summarizes the findings derived from the Power BI dashboard connected to the Snowflake data warehouse.  
The analysis focuses on overall profitability, regional performance, carrier efficiency, and product trends.

---

### Revenue and Profit Overview
- Total revenue reached **$48.17M**, with total profit of **$23.67M** and total cost of **$24.50M**.  
- Approximately **139K units** were sold across all product categories.  
- The business maintained an average **profit margin of 49%**, reflecting efficient pricing and cost management.  
- Sales and profitability trends indicate strong operational efficiency and financial performance across regions.

---

### Regional Performance
- **Toronto, Brampton, and Vancouver** recorded the highest total revenue.  
- **Moncton and Sudbury** showed lower sales, suggesting regional demand gaps.  
- Revenue is heavily concentrated in urban markets, where marketing and logistics operations are most effective.  
- Expanding marketing campaigns and logistics infrastructure in mid-tier cities can help balance performance and tap new customer bases.

---

### Shipment Carrier Efficiency
- **Intelcom** handled about 44% of all shipments, making it the most utilized and efficient carrier.  
- **Purolator** and **AMZL** each handled roughly 7% of total shipments.  
- **UPS, Canada Post, and FedEx** collectively managed the remaining 28%.  
- Intelcom’s dominance demonstrates operational strength but also creates a single-carrier dependency risk.  
- Increasing partnerships with secondary carriers can improve delivery flexibility and reduce potential disruptions.

---

### Product Category Analysis
- **High-profit categories** include Electronics, Home & Kitchen, and Grocery.  
- **Lower-margin categories** such as Books and Pet Supplies maintained steady sales but contributed less to overall profitability.  
- High-value categories performed best in major cities with higher income levels.  
- Prioritizing promotions for high-margin categories and bundling slower-moving items can improve total profitability.

---

### Monthly Performance Trends
- Profit peaked in **July**, indicating a strong mid-year sales period.  
- A slight decline in **August and September** suggests seasonal variation or post-promotional slowdown.  
- The data highlights a clear mid-year sales spike associated with promotional or holiday activity.  
- These patterns can be used for **sales forecasting**, **inventory optimization**, and **marketing planning** during high-demand months.

---

### Overall Business Insight
- Urban regions are the primary revenue drivers, supported by efficient logistics and strong product demand.  
- High-margin categories contribute most significantly to profitability.  
- Intelcom’s strong delivery performance supports sales but introduces dependency risk.  
- Strategic actions such as diversifying carrier networks, strengthening marketing in smaller regions, and focusing on profitable products could increase overall profit margins by an estimated **10–15%** in the next sales cycle.

---


### Key Metrics Implemented in Power BI
Average Unit Price = AVERAGE(FACT_SALES[UNIT_PRICE])  
Average Unit Cost = AVERAGE(FACT_SALES[UNIT_COST])  
Profit Margin = (Average Unit Price - Average Unit Cost) / Average Unit Price  
These dynamic DAX-based measures allow accurate tracking of price efficiency, profitability, and regional performance.

### Visualization Summary
The Power BI dashboard includes:
- Revenue by City chart to compare regional performance.
- Units Sold by Carrier chart to assess shipping efficiency.
- Monthly Profit Trend line chart to show seasonal variation.
- Profit vs. Unit Cost scatter plot to highlight cost-performance relationships.

Each visual connects directly to Snowflake data for live querying and real-time updates, ensuring that metrics always reflect the latest data.

### Conclusion
This project demonstrates how a fully integrated data pipeline using Python, Snowflake, and Power BI can convert raw sales data into actionable insights. By cleaning and transforming data in Python, modeling it through a Snowflake star schema for scalability, and visualizing real-time metrics in Power BI with DAX-based measures, the workflow delivers a complete view of sales performance, profitability, and logistics efficiency. This end-to-end approach highlights the ability to design modern, cloud-based analytics solutions.

