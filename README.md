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

## Data Insights and Analysis

### Revenue and Profit Overview
- **Total Revenue:** $48.17M  
- **Total Profit:** $23.67M  
- **Total Cost:** $24.50M  
- **Units Sold:** 139K  
- Indicates strong cost efficiency and consistent revenue generation across regions.  
- Sales performance remains steady with clear profitability trends.

---

### Regional Performance
- **Toronto**, **Brampton**, and **Vancouver** generated the highest revenue.  
- **Moncton** and **Sudbury** recorded lower sales volumes.  
- Most revenue comes from large metropolitan areas where logistics are strongest.  
- Expanding digital marketing to mid-tier cities could drive additional growth opportunities.

---

### Shipment Carrier Efficiency
- **Intelcom** handled around 44% of total shipments, making it the top-performing carrier.  
- **Purolator** and **AMZL** managed approximately 7% each, while **UPS**, **Canada Post**, and **FedEx** covered the remaining 28%.  
- Intelcom’s dominance highlights strong operational efficiency but also dependence on a single carrier.  
- Broadening carrier partnerships would increase flexibility and reduce delivery risks.

---

### Product Category Analysis
- **High-performing categories:** Electronics, Home & Kitchen, and Grocery.  
- Categories with higher **average unit prices** showed stronger revenue contribution.  
- Focusing on high-value items and optimizing pricing strategies can enhance overall returns.  
- The Power BI scatter plot confirmed a positive trend between unit price and total profit by category.

---

### Monthly Performance Trends
- Profit peaked in **July**, followed by a slight decline in **August–September**.  
- Mid-year spikes align with potential promotional or seasonal activity.  
- These insights can guide inventory planning and marketing timing for future campaigns.

---

### Overall Business Insights
- Urban regions drive the majority of sales and profitability.  
- High-value product categories remain the main revenue contributors.  
- Intelcom ensures fast and efficient logistics but creates dependency.  
- Expanding regional reach and carrier diversity could improve long-term stability and performance.

---

## Key Power BI Metrics
- **Average Unit Price:**  
  `AVERAGE(FACT_SALES[UNIT_PRICE])`
- **Average Unit Cost:**  
  `AVERAGE(FACT_SALES[UNIT_COST])`

These DAX measures provide insight into pricing efficiency and cost control, directly connected to the Snowflake fact table for live data analysis.

---

## Dashboard Summary
The Power BI dashboard visualizes:
- **Revenue by City** – Regional performance comparison.  
- **Units Sold by Carrier** – Shipment distribution across partners.  
- **Monthly Profit Trend** – Seasonality and performance shifts.  
- **Profit vs. Unit Cost (Scatter)** – Relationship between pricing and cost at the category level.  

All visuals are dynamically connected to Snowflake for real-time analytics and consistent data updates.

---

## Conclusion
This project demonstrates an end-to-end analytics workflow built with **Python**, **Snowflake**, and **Power BI**. From cleaning and preparing raw data to designing a Snowflake star schema and building interactive Power BI visuals, the process provides a clear, data-driven understanding of sales, costs, and logistics efficiency.  It showcases the ability to structure, model, and visualize data effectively to support business insights and decision-making.
