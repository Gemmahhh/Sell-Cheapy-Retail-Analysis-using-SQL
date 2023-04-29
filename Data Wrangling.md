# <p align="center" style="margin-top: 0px;"> :boom: SELL CHEAPY RETAIL ANALYSIS :boom:
## <p align="center"> Data Wrangling 

### The sole purpose of this section is to get the tables necessary for the analysis ready 

#### 1. Extracting the information embeded in the Demographics column found in the Person table

#### Steps Taken:

* First, a new column [Demographics_string] was created using ALTER and UPDATE. This new column converts the 'Demographics' column which was stored as an xml variable to string
* Then using CAST and SUBSTRING the details were extracted and stored in different variables, some as integers and others as strings. 

```sql
	  ALTER TABLE [AdventureWorks2019].[Sales].[Store] ADD demographics_string NVARCHAR(MAX);
  --dont run them both together(alter table and update)
    UPDATE [AdventureWorks2019].[Sales].[Store]
    SET demographics_string = CAST([AdventureWorks2019].[Sales].[Store].[Demographics] as nvarchar(max))
    FROM [AdventureWorks2019].[Sales].[Store]

-- Now from the 'demographics_string' column, we can now extract some relevant information we need for this analysis
SELECT 
		[BusinessEntityID],
    [Name],
    [SalesPersonID],
		[ModifiedDate],
	  CAST(SUBSTRING(demographics_string, CHARINDEX('<AnnualSales>', demographics_string) + LEN('<AnnualSales>'), CHARINDEX('</AnnualSales>', demographics_string) - CHARINDEX('<AnnualSales>', demographics_string) - LEN('<AnnualSales>')) AS INT) AS AnnualSales,
		CAST(SUBSTRING(demographics_string, CHARINDEX('<AnnualRevenue>', demographics_string) + LEN('<AnnualRevenue>'), CHARINDEX('</AnnualRevenue>', demographics_string) - CHARINDEX('<AnnualRevenue>', demographics_string) - LEN('<AnnualRevenue>')) AS INT) AS AnnualRevenue,
		SUBSTRING(demographics_string, CHARINDEX('<BankName>', demographics_string) + LEN('<BankName>'), CHARINDEX('</BankName>', demographics_string) - CHARINDEX('<BankName>', demographics_string) - LEN('<BankName>')) AS BankName,
		SUBSTRING(demographics_string, CHARINDEX('<BusinessType>', demographics_string) + LEN('<BusinessType>'), CHARINDEX('</BusinessType>', demographics_string) - CHARINDEX('<BusinessType>', demographics_string) - LEN('<BusinessType>')) AS BusinessType,
		CAST(SUBSTRING(demographics_string, CHARINDEX('<YearOpened>', demographics_string) + LEN('<YearOpened>'), CHARINDEX('</YearOpened>', demographics_string) - CHARINDEX('<YearOpened>', demographics_string) - LEN('<YearOpened>')) AS INT) AS YearOpened,
		SUBSTRING(demographics_string, CHARINDEX('<Specialty>', demographics_string) + LEN('<Specialty>'), CHARINDEX('</Specialty>', demographics_string) - CHARINDEX('<Specialty>', demographics_string) - LEN('<Specialty>')) AS Specialty,
		CAST(SUBSTRING(demographics_string, CHARINDEX('<SquareFeet>', demographics_string) + LEN('<SquareFeet>'), CHARINDEX('</SquareFeet>', demographics_string) - CHARINDEX('<SquareFeet>', demographics_string) - LEN('<SquareFeet>')) AS INT) AS SquareFeet,
		SUBSTRING(demographics_string, CHARINDEX('<Brands>', demographics_string) + LEN('<Brands>'), CHARINDEX('</Brands>', demographics_string) - CHARINDEX('<Brands>', demographics_string) - LEN('<Brands>')) AS Brands,
		SUBSTRING(demographics_string, CHARINDEX('<Internet>', demographics_string) + LEN('<Internet>'), CHARINDEX('</Internet>', demographics_string) - CHARINDEX('<Internet>', demographics_string) - LEN('<Internet>')) AS Internet,
		CAST(SUBSTRING(demographics_string, CHARINDEX('<NumberEmployees>', demographics_string) + LEN('<NumberEmployees>'), CHARINDEX('</NumberEmployees>', demographics_string) - CHARINDEX('<NumberEmployees>', demographics_string) - LEN('<NumberEmployees>')) AS INT) AS NumberEmployees
		

FROM [AdventureWorks2019].[Sales].[Store]
```


#### THE OUTPUT:
BusinessEntityID |	Name |	SalesPersonID | ModifiedDate	| AnnualSales	| AnnualRevenue |	BankName |	BusinessType |	YearOpened |	Specialty |	SquareFeet |	Brands |	Internet |	NumberEmployees
-- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- | -- 
292 |	Next-Door Bike Store |	279 | 2014-09-12 11:15:07.497 |	800000 |	80000 |	United Security |	BM |	1996 |	Mountain |	21000 |	2 |	ISDN |	13
294 |	Professional Sales and Service |	276 | 2014-09-12 11:15:07.497 |	800000 |	80000 |	International Bank |	BM |	1991 |	Touring |	18000 |	4+ |	T1 |	14
296 |	Riders Company |	277 | 2014-09-12 11:15:07.497 |	800000 |	80000 |	Primary Bank &amp; Reserve |	BM |	1999 |	Road |	21000 |	2 |	DSL |	15
298 |	The Bike Mechanics |	275 | 2014-09-12 11:15:07.497 |	800000 |	80000 |	International Security |	BM |	1994 |	Mountain |	18000 |	2 |	DSL |	16
300 |	Nationwide Supply |	286 |2014-09-12 11:15:07.497 |	800000 |	80000 |	Guardian Bank |	BM |	1987 |	Touring |	21000 |	4+ |	DSL |	17
302 |	Area Bike Accessories |	281 |2014-09-12 11:15:07.497 |	300000 |	30000 |	International Bank |	BM |	1982 |	Road |	9000 |	AW |	T2 |	8
304 |	Bicycle Accessories and Kits |	283 | 2014-09-12 11:15:07.497 |	300000 |	30000 |	Primary Bank &amp; Reserve |	BM |	1990 |	Mountain |	7000 |	AW |	T1 |	9
306 |	Clamps & Brackets Co. |	275 | 2014-09-12 11:15:07.497 |	800000 |	80000 |	International Security |	BM |	1985 |	Mountain |	17000 |	4+ |	DSL |	10

* This isn't the entire output

---

#### 2. Merging the Transaction History Table and the Transaction History Archive table together for it to be used to find out the total number of transactions during exploration

#### Step Taken:
* A CTE called 'cte_transaction' which uses UNION to join the both tables was created 

```sql
WITH cte_transaction AS (
SELECT *
FROM [AdventureWorks2019].[Production].[TransactionHistoryArchive]

UNION
SELECT *
FROM [AdventureWorks2019].[Production].[TransactionHistory]
)
SELECT *
FROM cte_transaction
```

#### THE OUTPUT:
TransactionID |	ProductID |	ReferenceOrderID |	ReferenceOrderLineID |	TransactionDate |	TransactionType |	Quantity |	ActualCost |	ModifiedDate
-- | -- | -- | -- | -- | -- | -- | -- | -- 
1 |	1 |	1 |	1 |	2011-04-16 00:00:00.000 |	P |	4 |	50.26 |	2011-04-16 00:00:00.000
2 |	359 |	2 |	1 |	2011-04-16 00:00:00.000 |	P |	3 |	45.12 |	2011-04-16 00:00:00.000
3 |	360 |	2 |	2 |	2011-04-16 00:00:00.000 |	P |	3 |	45.5805 |	2011-04-16 00:00:00.000
4 |	530 |	3 |	1 |	2011-04-16 00:00:00.000 |	P |	550 |	16.086 |	2011-04-16 00:00:00.000
5 |	4 |	4 |	1 |	2011-04-16 00:00:00.000 |	P |	3 |	57.0255 |	2011-04-16 00:00:00.000

* This isn't the entire output

---

#### 3. Creating a new column in the Work Order table used to find out the average product delay days 

#### Steps Taken:
* A new column was created in the Work Order table 
* Using DATEDIFF, the Due date was subtracted by the End date 

```sql
  ALTER TABLE [AdventureWorks2019].[Production].[WorkOrder] ADD delay_days INT;
  --dont run them both together(alter table and update)
  UPDATE [AdventureWorks2019].[Production].[WorkOrder]
  SET delay_days = DATEDIFF(day, [DueDate], [EndDate])

  SELECT delay_days
  FROM [AdventureWorks2019].[Production].[WorkOrder]
```
#### THE OUTPUT:

delay_days |
-- |
-1 |
-1 |
-1 |
-1 |
-1 |
-1 |
-1 |
-1 |
-1 |
-1 |

* Negative values means that the production ended on time (i.e '-1' simply means that the production ended one day before it was due).
* Positive values means that there was a delay in production (i.e '5' days means that the production ended 5 days after it was due). 
* This isn't the entire output

---

#### 4. Filtering down the joined transation table to 'Sales Transaction' only which would be used for the Sales trend analysis 
* For this, a view (Sales_Transaction_Type] was created that filtered down the joined transaction talble in order to display just Sales Transactions. 
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

#### THE OUTPUT:

TransactionID |	ProductID |	ReferenceOrderID |	ReferenceOrderLineID |	TransactionDate |	TransactionType |	Quantity |	ActualCost |	ModifiedDate
-- | -- | -- | -- | -- | -- | -- | -- | -- 
16 |	709 |	43659 |	10 |	2011-05-31 00:00:00.000 |	S |	6 |	6.175 |	2011-05-31 00:00:00.000
17 |	711 |	43659 |	12 |	2011-05-31 00:00:00.000 |	S |	4 |	20.1865 |	2011-05-31 00:00:00.000
18 |	712 |	43659 |	11 |	2011-05-31 00:00:00.000 |	S |	2 |	5.6187 |	2011-05-31 00:00:00.000
19 |	714 |	43659 |	8 |	2011-05-31 00:00:00.000 |	S |	3 |	31.2437 |	2011-05-31 00:00:00.000
20 |	716 |	43659 |	9 |	2011-05-31 00:00:00.000 |	S |	1 |	31.2437 |	2011-05-31 00:00:00.000

* This isn't the entire output















