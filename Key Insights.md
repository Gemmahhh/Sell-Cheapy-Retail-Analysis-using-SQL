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
* This trend can be seen properly in the dashboard shared in the ReadMe file
* The question now is, what could be the cause of this decline in sales?
  
---
  
### 2. What is Sell Cheapy Retail's hottest spot? What state province does Sell Cheapy Retail have the most amount of customers? 

### Steps Taken:
  
* Use the ORDER BY function to display the states provinces and number if customers in a descending order
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
* Did anything happen in Carlifonia to possibly change Sell Cheapy Retail's sales trend? 
  
---
  
### 3. What is the trend of Tax Rate in California over the years? 

### Steps Taken:
  
* Use the ORDER BY function to display the states provinces and number if customers in a descending order
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
  
![Top 5 state provinces](https://github.com/Gemmahhh/Sell-Cheapy-Retail-Analysis-using-SQL/blob/main/images/tax%20rate%20in%20CA%20over%20time.JPG)

* Sell Cheapy Retail's hottest state province is CALIFORNIA
* Did anything happen in Carlifonia to possibly change Sell Cheapy Retail's sales trend? 
  
---
  
### 4. What is the income range of most of Sell Cheapy Retail's customers? 

### Steps Taken:
  
* Use the ORDER BY function to display the states provinces and number if customers in a descending order
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
  
![Top 5 state provinces](https://github.com/Gemmahhh/Sell-Cheapy-Retail-Analysis-using-SQL/blob/main/images/tax%20rate%20in%20CA%20over%20time.JPG)

* Sell Cheapy Retail's hottest state province is CALIFORNIA
* Did anything happen in Carlifonia to possibly change Sell Cheapy Retail's sales trend?

  
  
  
