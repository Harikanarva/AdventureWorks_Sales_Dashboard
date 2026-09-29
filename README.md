# AdventureWorks Sales Dashboard

An interactive 5-page Power BI report that tracks sales, profit, returns, and customer behaviour for AdventureWorks, a global bike retailer, with target tracking and a price-sensitivity what-if model.

**Stack:** Power BI · Power Query  · DAX

---

## Business Problem

Leadership needs one place to answer:

1. How are revenue, profit, and orders trending against target?
2. Which product categories and regions drive the business, and where do returns hurt?
3. Which customer segments buy the most?
4. What happens to revenue and profit if prices change?

---

## Dashboard Preview

| Page | Preview |
|---|---|
| Exec Dashboard | `C:\Users\HP\Desktop\OneDrive\Pictures\Screenshots\Screenshot 2026-09-29 114238.png` |
| Map | `C:\Users\HP\Desktop\OneDrive\Pictures\Screenshots\Screenshot 2026-09-29 114309.png` |
| Product Detail | `C:\Users\HP\Desktop\OneDrive\Pictures\Screenshots\Screenshot 2026-09-29 114331.png` |
| Customer Detail | `C:\Users\HP\Desktop\OneDrive\Pictures\Screenshots\Screenshot 2026-09-29 114357.png` |

---
<img width="1313" height="735" alt="Screenshot 2026-09-29 114238" src="https://github.com/user-attachments/assets/4b291452-87df-4d8c-9f2d-7de59ca0480d" />
<img width="1312" height="731" alt="Screenshot 2026-09-29 114309" src="https://github.com/user-attachments/assets/118d8878-05e2-4409-ab36-8e6e7152fe18" />
<img width="1307" height="732" alt="Screenshot 2026-09-29 114331" src="https://github.com/user-attachments/assets/b5c2cbc7-3ab9-46ec-8d86-99490b58f99c" />
<img width="1308" height="732" alt="Screenshot 2026-09-29 114357" src="https://github.com/user-attachments/assets/ea9a2fa1-7fd8-41b6-9714-f10661d3aea4" />
<img width="540" height="277" alt="Screenshot 2026-09-29 114412" src="https://github.com/user-attachments/assets/9493eb4e-c23e-4ea8-908b-97271490b334" />


## Dataset

AdventureWorks sales data, loaded from CSV files: 1 Jan 2020 to 30 Jun 2022.

| Table | Rows | Description |
|---|---|---|
| Sales Data | 56,046 | Order lines (yearly files combined from a folder) |
| Returns Data | 1,809 | Returned units by date, product, territory |
| Customer Lookup | 18,148 | Demographics, income, occupation |
| Product Lookup | 293 | SKU, cost, price |
| Product Subcategories / Categories Lookup | 37 / 4 | Product hierarchy |
| Territory Lookup | 10 | Region, country, continent |
| Calendar Lookup | 912 | Date table |

---

## Data Model

Star schema with a snowflake product hierarchy. All relationships are many-to-one, single direction.

- `Sales Data` → Product, Customer, Territory, Calendar (`OrderDate`, active)
- `Returns Data` → Product, Territory, Calendar (`ReturnDate`)
- `Product Lookup` → Subcategories → Categories
- `StockDate` → Calendar is kept as an inactive relationship

**Tables built for the report:** `Measure Table` (holds all measures), `Price Adjustment (%)` (what-if parameter, -100% to +100%), `Product Metric Selection` and `Customer Metric Selection` (field-switching slicers), `Rolling Calendar`.

---

## Data Preparation (Power Query)

- Combined the yearly sales CSVs from a folder into one table
- Set data types; removed rows with missing customer keys
- Proper-cased customer names and built a full name column
- Extracted the email domain from customer email addresses
- Built SKU type, a 10% discount price, and date fields (day name, week, month, quarter, year) on the calendar

---

## DAX

**39 measures** and **15 calculated columns**.

| Group | Examples |
|---|---|
| Core KPIs | Total Orders, Total Customers, Total Revenue, Total Cost, Total Profit, Return Rate |
| Comparison | % of All Orders, % of All Returns (`ALL`), Bulk / Weekend / High-Ticket Orders, Bike Return Rate |
| Time intelligence | YTD Revenue (`DATESYTD`), Previous Month (`DATEADD`), 10-day Rolling Revenue, 90-day Rolling Profit (`DATESINPERIOD`) |
| Targets | Revenue / Profit / Order Target (previous month × 1.1) and gap to target |
| What-if | Adjusted Price, Adjusted Revenue, Adjusted Profit |
| Segmentation columns | Income Level, Customer Priority, Price Point, Education Category, Weekend flag |

Revenue is calculated as list price × quantity (`SUMX` with `RELATED`), so it excludes discounts and returns.

---

## Report Pages

| Page | What it answers |
|---|---|
| **Exec Dashboard** | KPI cards, revenue trend, monthly revenue / orders / returns, orders by category |
| **Map** | Performance by country and region |
| **Product Detail** | Gauges for orders, revenue, and profit vs target; profit and return trends; price what-if slicer; metric switcher |
| **Customer Detail** | Orders by income level and occupation, Top 100 customers table, metric switcher |
| **Category Tooltip** | Hidden hover page showing weekly orders by category |

Navigation uses custom buttons, and the metric selectors let one visual switch between measures.

---

## Key Insights

- **Scale:** $24.9M revenue, $10.5M profit (42% margin), 25,164 orders
- **Category concentration:** Bikes are about 95% of revenue but only 16.5% of units; accessories are 69% of units but 3.6% of revenue
- **Returns:** overall return rate is 2.2%. Bikes have the highest rate (3.1%), while accessories make up 62% of returned units
- **Geography:** the US and Australia bring in about 62% of revenue
- **Growth:** H1 2022 revenue ($9.19M) nearly matched all of 2021 ($9.32M)


---

## Who Would Use This

- **Executives:** monthly performance against target
- **Product and category managers:** pricing and returns
- **Regional sales heads:** territory performance
- **Marketing / CRM teams:** high-income and top-customer segments
- **Finance:** target gaps and price scenarios

---

## Repository Structure

```
adventureworks-sales-dashboard/
├── README.md
├── AdventureWorks-Sales-Dashboard.pbix
├── screenshots/
└── data/        
```

---

## How to Run

1. Open `AdventureWorks-Sales-Dashboard.pbix` in Power BI Desktop.
2. If the data won't refresh, go to **Home → Transform data → Data source settings** and repoint each source to your local copy of the CSV files.
3. Refresh and explore the pages.

---

## Limitations

- Revenue uses list price, so discounts and returns are not deducted
- Targets are a simple rule (previous month × 1.1), not a forecast
- The what-if scales the average price, which is only exact if prices are uniform
- Data ends in June 2022

---

## Author

**Harika** · Computer Science Engineering graduate (2026), aspiring data analyst
GitHub: https://github.com/Harikanarva  · LinkedIn: https://linkedin.com/in/harika-narva-89305b246
