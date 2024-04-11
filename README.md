<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <!-- External CSS link, adjust the href attribute accordingly -->
  <link rel="stylesheet" href="styles.css">
</head>
<body>
  <header>
    <h1>Data Analyst Project – Sales Management</h1>
    <nav>
      <ul>
        <!-- You can add links to different sections of your project -->
        <li><a href="#about">About</a></li>
        <li><a href="#data">Data</a></li>
        <li><a href="#analysis">Analysis</a></li>
        <li><a href="#results">Results</a></li>
      </ul>
    </nav>
  </header>
  
  <main>
    <section id="about">
      <h2>About</h2>
      <p>The business request for this data analyst project was an executive sales report for sales managers. Based on the request that was made from the business we following user stories were defined to fulfill delivery and ensure that acceptance criteria’s were maintained throughout the project.</p>
      <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  
</head>
<body>

<table>
  <thead>
    <tr>
      <th>#</th>
      <th>As a</th>
      <th>I want</th>
      <th>So that</th>
      <th>Acceptance Criteria</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>1</td>
      <td>Sales Manager</td>
      <td>To get a dashboard overview of internet sales</td>
      <td>Can follow better which customers and products sells the best</td>
      <td>A Power BI dashboard which updates data once a day</td>
    </tr>
    <tr>
      <td>2</td>
      <td>Sales Representative</td>
      <td>A detailed overview of Internet Sales per Customers</td>
      <td>Can follow up my customers that buys the most and who we can sell more to</td>
      <td>A Power BI dashboard which allows me to filter data for each customer</td>
    </tr>
    <tr>
      <td>3</td>
      <td>Sales Representative</td>
      <td>A detailed overview of Internet Sales per Products</td>
      <td>Can follow up my Products that sells the most</td>
      <td>A Power BI dashboard which allows me to filter data for each Product</td>
    </tr>
    <tr>
      <td>4</td>
      <td>Sales Manager</td>
      <td>A dashboard overview of internet sales</td>
      <td>Follow sales over time against budget</td>
      <td>A Power BI dashboard with graphs and KPIs comparing against budget.</td>
    </tr>
  </tbody>
</table>

</body>
</html>
</section>

  <section id="data">
      <h2>Data</h2>
      <p>DTo create the necessary data model for doing analysis and fulfilling the business needs defined in the user stories the following tables were extracted using SQL.

One data source (sales budgets) were provided in Excel format and were connected in the data model in a later step of the process.

Below are the SQL statements for cleansing and transforming necessary data.</p>
    <!DOCTYPE html>
    </section>
    <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
 
<p><b><u>DIM_Calendar:</u></b></p>
</head>
<body>

<pre>
  <code>
    -- Cleansed DIM_Date Table --
SELECT 
  [DateKey], 
  [FullDateAlternateKey] AS Date, 
  --[DayNumberOfWeek], 
  [EnglishDayNameOfWeek] AS Day, 
  --[SpanishDayNameOfWeek], 
  --[FrenchDayNameOfWeek], 
  --[DayNumberOfMonth], 
  --[DayNumberOfYear], 
  --[WeekNumberOfYear],
  [EnglishMonthName] AS Month, 
  Left([EnglishMonthName], 3) AS MonthShort,   -- Useful for front end date navigation and front end graphs.
  --[SpanishMonthName], 
  --[FrenchMonthName], 
  [MonthNumberOfYear] AS MonthNo, 
  [CalendarQuarter] AS Quarter, 
  [CalendarYear] AS Year --[CalendarSemester], 
  --[FiscalQuarter], 
  --[FiscalYear], 
  --[FiscalSemester] 
FROM 
 [AdventureWorksDW2019].[dbo].[DimDate]
WHERE 
  CalendarYear >= 2019
  </code>
</pre>

</body>
</html>

<p><b><u>DIM_Customers:</u></b></p>
</head>
<body>

<pre>
  <code>
   -- Cleansed DIM_Customers Table --
SELECT 
  c.customerkey AS CustomerKey, 
  --      ,[GeographyKey]
  --      ,[CustomerAlternateKey]
  --      ,[Title]
  c.firstname AS [First Name], 
  --      ,[MiddleName]
  c.lastname AS [Last Name], 
  c.firstname + ' ' + lastname AS [Full Name], 
  -- Combined First and Last Name
  --      ,[NameStyle]
  --      ,[BirthDate]
  --      ,[MaritalStatus]
  --      ,[Suffix]
  CASE c.gender WHEN 'M' THEN 'Male' WHEN 'F' THEN 'Female' END AS Gender,
  --      ,[EmailAddress]
  --      ,[YearlyIncome]
  --      ,[TotalChildren]
  --      ,[NumberChildrenAtHome]
  --      ,[EnglishEducation]
  --      ,[SpanishEducation]
  --      ,[FrenchEducation]
  --      ,[EnglishOccupation]
  --      ,[SpanishOccupation]
  --      ,[FrenchOccupation]
  --      ,[HouseOwnerFlag]
  --      ,[NumberCarsOwned]
  --      ,[AddressLine1]
  --      ,[AddressLine2]
  --      ,[Phone]
  c.datefirstpurchase AS DateFirstPurchase, 
  --      ,[CommuteDistance]
  g.city AS [Customer City] -- Joined in Customer City from Geography Table
FROM 
  [AdventureWorksDW2019].[dbo].[DimCustomer] as c
  LEFT JOIN dbo.dimgeography AS g ON g.geographykey = c.geographykey 
ORDER BY 
  CustomerKey ASC -- Ordered List by CustomerKey
  </code>
</pre>

</body>
</html>


</body>
</html>

<p><b><u>DIM_Products:</u></b></p>
</head>
<body>

<pre>
  <code>
  -- Cleansed DIM_Products Table --
SELECT 
  p.[ProductKey], 
  p.[ProductAlternateKey] AS ProductItemCode, 
  --      ,[ProductSubcategoryKey], 
  --      ,[WeightUnitMeasureCode]
  --      ,[SizeUnitMeasureCode] 
  p.[EnglishProductName] AS [Product Name], 
  ps.EnglishProductSubcategoryName AS [Sub Category], -- Joined in from Sub Category Table
  pc.EnglishProductCategoryName AS [Product Category], -- Joined in from Category Table
  --      ,[SpanishProductName]
  --      ,[FrenchProductName]
  --      ,[StandardCost]
  --      ,[FinishedGoodsFlag] 
  p.[Color] AS [Product Color], 
  --      ,[SafetyStockLevel]
  --      ,[ReorderPoint]
  --      ,[ListPrice] 
  p.[Size] AS [Product Size], 
  --      ,[SizeRange]
  --      ,[Weight]
  --      ,[DaysToManufacture]
  p.[ProductLine] AS [Product Line], 
  --     ,[DealerPrice]
  --      ,[Class]
  --      ,[Style] 
  p.[ModelName] AS [Product Model Name], 
  --      ,[LargePhoto]
  p.[EnglishDescription] AS [Product Description], 
  --      ,[FrenchDescription]
  --      ,[ChineseDescription]
  --      ,[ArabicDescription]
  --      ,[HebrewDescription]
  --      ,[ThaiDescription]
  --      ,[GermanDescription]
  --      ,[JapaneseDescription]
  --      ,[TurkishDescription]
  --      ,[StartDate], 
  --      ,[EndDate], 
  ISNULL (p.Status, 'Outdated') AS [Product Status] 
FROM 
  [AdventureWorksDW2019].[dbo].[DimProduct] as p
  LEFT JOIN dbo.DimProductSubcategory AS ps ON ps.ProductSubcategoryKey = p.ProductSubcategoryKey 
  LEFT JOIN dbo.DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey 
order by 
  p.ProductKey asc
  </code>
</pre>

</body>
</html>

</body>
</html>

<p><b><u>FACT_InternetSales:</u></b></p>
</head>
<body>

<pre>
  <code>
  -- Cleansed FACT_InternetSales Table --
SELECT 
  [ProductKey], 
  [OrderDateKey], 
  [DueDateKey], 
  [ShipDateKey], 
  [CustomerKey], 
  --  ,[PromotionKey]
  --  ,[CurrencyKey]
  --  ,[SalesTerritoryKey]
  [SalesOrderNumber], 
  --  [SalesOrderLineNumber], 
  --  ,[RevisionNumber]
  --  ,[OrderQuantity], 
  --  ,[UnitPrice], 
  --  ,[ExtendedAmount]
  --  ,[UnitPriceDiscountPct]
  --  ,[DiscountAmount] 
  --  ,[ProductStandardCost]
  --  ,[TotalProductCost] 
  [SalesAmount] --  ,[TaxAmt]
  --  ,[Freight]
  --  ,[CarrierTrackingNumber] 
  --  ,[CustomerPONumber] 
  --  ,[OrderDate] 
  --  ,[DueDate] 
  --  ,[ShipDate] 
FROM 
  [AdventureWorksDW2019].[dbo].[FactInternetSales]
WHERE 
  LEFT (OrderDateKey, 4) >= YEAR(GETDATE()) -2 -- Ensures we always only bring two years of date from extraction.
ORDER BY
  OrderDateKey ASC
  </code>
</pre>

</body>    
 
  <section id="analysis">
      <h2>Analysis</h2>
      <p>Below is a screenshot of the data model after cleansed and prepared tables were read into Power BI.

This data model also shows how FACT_Budget hsa been connected to FACT_InternetSales and other necessary DIM tables.</p>
    </section>
    <!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

</head>
<body>
<img src="https://drive.google.com/uc?id=12lVLv9ZuLj2wOVLrovjo4XCsmdboExLX" alt="Image from Google Drive" width="700">

</body>
</html>


<section id="results">
      <h2>Results</h2>
      <p>The finished sales management dashboard with one page with works as a dashboard and overview, with two other pages focused on combining tables for necessary details and visualizations to show sales over time, per customers and per products.</p>
    </section>
    
</head>
<body>
<img src="https://drive.google.com/uc?id=1nt3XF9IR4SoBCJOzA84WEB6fteq6okCA" alt="Image from Google Drive" width="700">
</body>
</html>

  </main>
  
  <footer>
    <p>&copy; KP KARTHIK</p>
  </footer>


</body>
</html>
