EV SQL Project README
======================

Files:
- EV_Sales_12000.csv: EV sales records (~12,000 rows). Columns: Year, Country, Manufacturer, Sales_Million
- Charging_Stations_4000.csv: Charging stations (~4,000 rows). Columns: Year, Country, Public_Chargers
- Region_Mapping.csv: Region mapping. Columns: Country, Region

How to import into MySQL (example):
1) Create tables in your database matching the schema below or use the provided SQL script.
2) Use LOAD DATA INFILE or your GUI to import CSV files.

Table schemas:
CREATE TABLE EV_Sales (Year INT, Country VARCHAR(100), Manufacturer VARCHAR(100), Sales_Million FLOAT);
CREATE TABLE Charging_Stations (Year INT, Country VARCHAR(100), Public_Chargers INT);
CREATE TABLE Region_Mapping (Country VARCHAR(100), Region VARCHAR(100));

Sample queries (to reproduce charts):
1) Total EV Sales by Year:
SELECT Year, ROUND(SUM(Sales_Million),4) AS Total_Sales_Million FROM EV_Sales GROUP BY Year ORDER BY Year;

2) Regional market share (2024):
SELECT r.Region, ROUND(SUM(s.Sales_Million),4) AS Regional_Sales FROM EV_Sales s JOIN Region_Mapping r ON s.Country = r.Country WHERE s.Year = 2024 GROUP BY r.Region;

3) Top manufacturers in 2024:
SELECT Manufacturer, SUM(Sales_Million) AS Sales_2024 FROM EV_Sales WHERE Year = 2024 GROUP BY Manufacturer ORDER BY Sales_2024 DESC LIMIT 10;

4) Charging stations by year:
SELECT Year, SUM(Public_Chargers) AS Total_Chargers FROM Charging_Stations GROUP BY Year ORDER BY Year;

5) EV Sales vs Chargers by Year:
SELECT s.Year, ROUND(SUM(s.Sales_Million),4) AS EV_Sales_Million, SUM(c.Public_Chargers) AS Total_Chargers FROM EV_Sales s JOIN Charging_Stations c ON s.Year = c.Year AND s.Country = c.Country GROUP BY s.Year ORDER BY s.Year;

Projection method:
- Projections in the PPT use CAGR computed from 2019 to 2024 applied to the 2024 baseline to forecast 2025-2030.

Credits:
- Data simulated to match IEA/EV-Volumes trends; intended for educational SQL/analytics projects.