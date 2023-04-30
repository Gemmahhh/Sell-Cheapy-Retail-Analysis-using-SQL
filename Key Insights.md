# <p align="center" style="margin-top: 0px;"> 💥 Sell Cheapy Retail Analysis 💥
## <p align="center"> Key Insights
  
### 1. What is Sell Cheapy Retail's Sales transaction trend? Has Sales been improving over the years or not? 

### Steps Taken:
  
* Create a view 'Sales_Transaction_Type' that joins the 'Transaction History' and 'Transaction History Archive' tables then filters the output to display sales transactions only
* Plot the output on Tableau (line chart) to see the trend

```sql
  CREATE VIEW [Sales_Transaction_Type] 
  AS
	SELECT *
	FROM [AdventureWorks2019].[Production].[TransactionHistoryArchive]
	WHERE [TransactionType] = 'S'
	UNION
	SELECT *
	FROM [AdventureWorks2019].[Production].[TransactionHistory]
	WHERE [TransactionType] = 'S'

	SELECT *
	FROM [Sales_Transaction_Type] 
```
### Output:
TransactionID |	ProductID |	ReferenceOrderID |	ReferenceOrderLineID |	TransactionDate |	TransactionType |	Quantity |	ActualCost |	ModifiedDate
-- | -- | -- | -- | -- | -- | -- | -- | -- 
16 |	709 |	43659 |	10 |	2011-05-31 00:00:00.000 |	S |	6 |	6.175 |	2011-05-31 00:00:00.000
17 |	711 |	43659 |	12 |	2011-05-31 00:00:00.000 |	S |	4 |	20.1865 |	2011-05-31 00:00:00.000
18 |	712 |	43659 |	11 |	2011-05-31 00:00:00.000 |	S |	2 |	5.6187 |	2011-05-31 00:00:00.000
19 |	714 |	43659 |	8 |	2011-05-31 00:00:00.000 |	S |	3 |	31.2437 |	2011-05-31 00:00:00.000
20 |	716 |	43659 |	9 |	2011-05-31 00:00:00.000 |	S |	1 |	31.2437 |	2011-05-31 00:00:00.000
  
![Sales Trend](https://github.com/Gemmahhh/Sell-Cheapy-Retail-Analysis-using-SQL/blob/main/images/sales%20trend.JPG)

* From the month of May 2013 they has been a gradual decrease in sales for Sell Cheapy Retail
* The question now is, what could be the cause of this decline in sales?
  
---
  
### 2. What is Sell Cheapy Retail's hottest spot? What state province does Sell Cheapy Retail have the most amount of customers? 

### Steps Taken:
  
* Use the ORDER BY function to display the states provinces and number of customers in a descending order
* Plot the output on tableau using a barchart

```sql
SELECT [StateProvinceCode],
       [number_of_customers]
  FROM [State_province]
  ORDER BY [number_of_customers] DESC

```
### Output:
StateProvinceCode |	number_of_customers |
-- | -- |
CA | 	5884 |
BC |	3472 |
ENG |	3219 |
WA | 	3042 |
NSW |	3009 |
  
![Top 5 state provinces](https://github.com/Gemmahhh/Sell-Cheapy-Retail-Analysis-using-SQL/blob/main/images/Top%205%20state%20provinces.JPG)

* Sell Cheapy Retail's hottest state province is CALIFORNIA
* Did anything happen in California to possibly change Sell Cheapy Retail's sales trend? 
  
---
  
### 3. What has been the trend of Tax Rate in California over the years? 

### Steps Taken:
  
* Use the WHERE function to select only California (Territory ID = 6) 
* Plot the output on tableau
	
```sql
  SELECT [OrderDate],
       [TaxAmt]
  FROM [Sales].[SalesOrderHeader]
  WHERE [TerritoryID] = 6
```
### Output:

OrderDate |	TaxAmt |
-- | -- |
2011-05-31 00:00:00.000 | 3153.7696 |
2011-05-31 00:00:00.000 | 2775.1646 |
2011-05-31 00:00:00.000 | 3461.7654 |
2011-05-31 00:00:00.000 |	587.6023 |
2011-05-31 00:00:00.000 |	251.9407 |
2011-05-31 00:00:00.000 |	747.1011 |
2011-05-31 00:00:00.000 |	125.8032 |
2011-05-31 00:00:00.000 |	286.2616 |
2011-06-18 00:00:00.000 |	286.2616 |
  
![Top 5 state provinces](https://github.com/Gemmahhh/Sell-Cheapy-Retail-Analysis-using-SQL/blob/main/images/tax%20rate%20in%20CA%20over%20time.JPG)

* The tax rate increased drastically in the month of April 2013, just a month before Sell Cheapy Retail's sales began to decline. 
* Territory ID '6' is California. 
* This is not the entire output. 
	
---
  
### 4. What is the income range of most of Sell Cheapy Retail's customers? 

### Steps Taken:
  
* Use the ORDER BY and GROUP BY function to display the Income range of customers in a descending order
* Plot the output on tableau using a barchart

```sql
    SELECT COUNT ([BusinessEntityID]) AS The_count,
       [YearlyIncome]
  FROM [Sales].[vPersonDemographics]
  GROUP BY YearlyIncome
  ORDER BY The_count DESC

```
### Output:

The_count |	YearlyIncome |
-- | -- |
5704 |	25001-50000 |
5476 |	50001-75000 |
2922 |	0-25000 |
2755 |	75001-100000 |
1627 |	greater than 100000 |
1488 |	NULL |
  
![Customers income](https://github.com/Gemmahhh/Sell-Cheapy-Retail-Analysis-using-SQL/blob/main/images/customer's%20income.JPG)

* The income range with the highest number of customers is '$25001 - $50000', next followed by '$50001 - $75000'
* Do customers from these ranges also generate the highest Average Transaction Value? 

---
	
### 5. Do customers that earn within '$25000 - $75000' generate the highest Average Transaction Value? 

### Steps Taken:
  
* This graph was plotted directly on Tableau using the appropriate table. 

![Customers income](https://github.com/Gemmahhh/Sell-Cheapy-Retail-Analysis-using-SQL/blob/main/images/ATV%20by%20income.JPG)

* Customers that earn within '$25001 - $75000' do not generate the highest amount of ATV. 
* The largest ATV is generated by those that earn above $100,000
  
  
  
