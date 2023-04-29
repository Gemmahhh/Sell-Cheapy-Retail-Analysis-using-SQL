# <p align="center" style="margin-top: 0px;"> 💥 SELL CHEAPY RETAIL ANALYSIS 💥
## <p align="center"> EXPLORING THE DATABASE
  
### 1. What is the total number of territories Sell Cheap Retail has branches situated in? 

### Step Taken:
  
* The DISTINCT function was used to find the number of unique territories

```sql
SELECT COUNT (DISTINCT [territoryID] ) AS Number_of_territories
FROM Sales.SalesTerritory
```
### Output:
Number_of_territories |
-- |
10 |

* Sell Cheapy Retail has stores in 10 different territories.
  
  ---
  
### 2. What are these territories?

### Steps Taken:
  
* Use the DISTINCT function to find the unique names of territories 

```sql
SELECT DISTINCT Name
FROM Sales.SalesTerritory
```
### Output:
  
Name |
-- |
Australia |
Canada |
Central |
France |
Germany |
Northeast |
Northwest |
Southeast |
Southwest |
United Kingdom |

* These are the 10 different territories Sell Cheapy Retail has stores situated in. 
  
---
  
### 3. What is the total number of countries regions the branches are situated in? 

### Steps Taken:
  
* Use the DISTINCT function to find the total number of unique countries

```sql
SELECT COUNT (DISTINCT [CountryRegionName]) AS Unique_countries
FROM [AdventureWorks2019].[Sales].[vIndividualCustomer]
```
### Output:
|Unique_countries|
| -- |
|6|

* Sell Cheapy Retail stores can be found in 10 different country regions. 
  
 ---
  
### 4. What are these countries?

### Steps Taken:
  
* Use the DISTINCT function to find the unique names of countries 

```sql
SELECT DISTINCT (CountryRegionName) AS Unique_countries
FROM [AdventureWorks2019].[Sales].[vIndividualCustomer]
```
### Output:
  
Unique_countries |
-- |
Australia |
Canada |
Germany |
France |
United Kingdom |
United States |

* These are the 6 different countries Sell Cheapy Retail has stores situated in. 
  
  ---
  
### 5. What is the total number of products sold by Sell Cheap Retail? 
  
### Steps Taken:
  
* Use the DISTINCT function to find the total number of products

```sql
SELECT COUNT(DISTINCT [Name]) AS Number_of_products
FROM [Production].[Product]
```
### Output:
  
Number_of_products |
-- |
504 |
  
* The total number of products is 504   
 
  
  ---
  
### 6. On an average is there usually delay in production? 


### Steps Taken:
  
* First, a column was created to hold the values of the number of delay days which was gotten by subtracting the Due date from the End date 
* Then, the using the AVG function, the average delay days was gotten 
* Using the STDEV function, the standard deviation of the delay days was also gotten 

```sql
  ALTER TABLE [AdventureWorks2019].[Production].[WorkOrder] ADD delay_days INT;
  --dont run them both together(alter table and update)
  UPDATE [AdventureWorks2019].[Production].[WorkOrder]
  SET delay_days = DATEDIFF(day, [DueDate], [EndDate])

  --lets see the average no of delay days
  SELECT AVG(delay_days) AS Average
FROM [AdventureWorks2019].[Production].[WorkOrder]

-- LETS SEE THE STANDARD DEVIATION OF THE DELAY DAYS 
SELECT STDEV(delay_days) AS Standard_deviation
FROM [Production].[WorkOrder]
```
### Output:
  
Average |
-- |
2|
  
Standard_deviation |
-- |
5.30038905199103|
  
* The average number of delay days is 2 and the standard deviation is 5.3

---
  
### 7. What is the total number of transactons?  
  
### Steps Taken:
  
* Create a cte 'cte_transaction' that tempoarily joins the Transaction History table and the Transaction History Archive table together
* Using this cte, the total number of transaction was then gotten using the COUNT function. 

```sql
WITH cte_transaction AS (
SELECT *
FROM [AdventureWorks2019].[Production].[TransactionHistoryArchive]

UNION
SELECT *
FROM [AdventureWorks2019].[Production].[TransactionHistory]
)

SELECT COUNT(DISTINCT [TRANSACTIONID]) AS Total_number_of_transactions
FROM cte_transaction

```
### Output:
  
Total_number_of_transactions |
-- |
202696 |
  
* The total number of transactions is 202696   
  
---
    
### 8. What is the most frequently purchased product sold by Sell Cheap Retail? 
  
### Steps Taken:
  
* First, the Product table and the Sales Order Detail tables were joined using the ProductID key 
* Then the output was grouped by the name of the products and ordered using the 'order_qty' variable

```sql
  SELECT [Name],
      SUM([AdventureWorks2019].[Sales].[SalesOrderDetail].[OrderQty]) AS order_qty
  FROM [AdventureWorks2019].[Sales].[SalesOrderDetail]
  INNER JOIN [AdventureWorks2019].[Production].[Product]
  ON [AdventureWorks2019].[Sales].[SalesOrderDetail].[ProductID] = [AdventureWorks2019].[Production].[Product].[ProductID]
  GROUP BY [Name]
  ORDER BY order_qty DESC
```
### Output:
  
Name |	order_qty |
-- | -- |
AWC Logo Cap |	8311 |
Water Bottle - 30 oz. |	6815 |
Sport-100 Helmet, Blue |	6743 |
Long-Sleeve Logo Jersey, L |	6592 |
Sport-100 Helmet, Black |	6532 |
Sport-100 Helmet, Red |	6266 |
Classic Vest, S |	4247 |
  
* These are the top 7 products 
* This is not the complete output 
  
---
  
### 9. What are the territories with the highest number of customers? 
  
### Steps Taken:
  
* A view 'customers_territory' was created
* This view joins the Customer table and the Sales Territory table using the TerritoryID
* After this was done the territories with the highest number of customers were then obtained using the GROUP BY clause and ORDER BY 

```sql
CREATE VIEW [Customers_territory]
AS
SELECT COUNT (DISTINCT CustomerID) AS Number_of_customers,
		[Name] AS Name_of_territory
FROM [AdventureWorks2019].[Sales].[Customer] AS Customer
INNER JOIN [AdventureWorks2019].[Sales].[SalesTerritory] AS territory
ON Customer.TerritoryID = territory.TerritoryID
GROUP BY [Name]

SELECT *
FROM [Customers_territory]
ORDER BY Number_of_customers DESC
```
### Output:
  
Number_of_customers |	Name_of_territory |
-- | -- |
4696 |	Southwest |
3665 |	Australia |
3520 |	Northwest |
1991 |	United Kingdom |
1884 |	France |
1852 |	Germany |
1791 |	Canada |
176 |	Southeast |
132 |	Central |
113 |	Northeast |
  
* Southwest is the territory with the highest number of customers
  
---
  
### 10. What are the stores with the highest number of customers? 
  
### Steps Taken:
  
* A view 'Hottest_stores' was created
* This view joins the Customer table and the Sales Territory table using the TerritoryID
* After this was done the stores with the highest number of customers were then obtained using the GROUP BY clause and ORDER BY 

```sql
    CREATE VIEW [Hottest_stores]
  AS
  SELECT [CustomerID]
      ,[StoreID]
      ,cust.[TerritoryID]
	  ,[Name]
      ,[CountryRegionCode]
      ,[Group]
FROM [AdventureWorks2019].[Sales].[Customer] AS cust
INNER JOIN [AdventureWorks2019].[Sales].[SalesTerritory] AS territory
ON cust.[TerritoryID] = territory.[TerritoryID]

SELECT StoreID,
	COUNT (DISTINCT CustomerID) AS Number_of_customers
FROM [Hottest_stores]
GROUP BY StoreID
ORDER BY Number_of_customers DESC
```
### Output:

StoreID |	Number_of_customers |
  -- | -- |
NULL |	18484 |
700 |	2 |
388 |	2 |
302 |	2 |
1868 |	2 |
1370 |	2 |
600 |	2 |
  
* Without the group by function, there were about 7 stores that had no unique store ID, this is why the number of customers in stores with 'NULL' as the store ID is extremely high
* However, this doesnt change the fact that most of the stores with large number of customers do not have unique IDs. 
* Most of the other stores had 2 or 1 customer(s) only. 
* This is not the complete output. 
  
---
 
### 11. Does Sell Cheapy Retail get higher number of customers when the tax rate increases OR NOT? 
  
### Steps Taken:
  
* First, a view 'Sales_data' was created
* This view groups the 'TaxAmt' column found in the Sales Header Order table using the WHEN and THEN clauses and stores the outpus as 1 through 8
* Then, another view 'tax_range' was created using the 'sales_data' view to count the number of sales for each tax range

```sql
CREATE VIEW [sales_data] 
AS 
SELECT [SalesOrderID],
[TaxAmt],
CASE
WHEN [TaxAmt] <= 300 THEN 1
WHEN [TaxAmt] <= 500 THEN 2
WHEN [TaxAmt] <= 1000 THEN 3
WHEN [TaxAmt] <= 2000 THEN 4
WHEN [TaxAmt] <= 3000 THEN 5
WHEN [TaxAmt] <= 5000 THEN 6
WHEN [TaxAmt] <= 10000 THEN 7
WHEN [TaxAmt] <= 15000 THEN 8
ELSE 9
END AS tax_range
FROM [AdventureWorks2019].[Sales].[SalesOrderHeader]


CREATE VIEW [tax_range]
AS
SELECT
COUNT(DISTINCT [SalesOrderID]) AS number_of_sales,
CONCAT((tax_range - 1) * 500 + 1, '-', tax_range * 500) AS the_range
FROM sales_data
GROUP BY tax_range


SELECT *
FROM [tax_range]
ORDER BY number_of_sales DESC
```
### Output:

number_of_sales |	the_range
  -- | -- |
28952 |	1-500 |
625 |	2501-3000 |
420 |	3001-3500 |
411 |	2001-2500 |
381 |	1001-1500 |
326 |	501-1000 |
302 |	1501-2000 |
46 |	3501-4000 |
2 |	4001-4500 |
  
* As seen from the table above, as the tax range increases the number of sales reduces 
  
  
