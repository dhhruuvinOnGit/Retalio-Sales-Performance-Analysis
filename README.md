# Retalio-Sales-Performance-Analysis
This project gives insights on sales of a fictitious company - Retalio.

### Dashboard Link : https://app.powerbi.com/groups/me/reports/5b13566f-21b6-4be7-8e71-e4b66113c6dc/5daf80920dacbc221723?redirectedFromSignup=1&experience=power-bi

### Overview :
This dashboard, titled "Retalio Sales Performance Report," provides a comprehensive analysis of sales performance over a specified period. It offers various visualizations and key metrics that help understand the sales trends, product performance, and regional sales distribution.

### Steps followed :
#### Data Transformation:
- Step 1: Load the dataset into PowerBi Desktop. Click on 'Transform Data' to clean and transform the dataset.
- Step 2: Change datatype of all columns based on their values in dataset.
- Step 3: Duplicate the 'Order Date' column and extract year from it and rename the duplicated column to 'Order Date Year' and set datatype to Date format.
- Step 4: Select 'Close & Apply' to apply the changes made.

#### Data Visualization:
- Create New Measures/Table/Column from following DAX script:
1. Profit LY = CALCULATE([Total Profit], DATEADD('Date'[Date], -1, YEAR))
2. Profit YoY% = 
    IF(
	    ISFILTERED('Date'[Date]),
	    ERROR("Time intelligence quick measures can only be grouped or filtered by the Power BI-provided date hierarchy or primary date column."),
	    VAR __PREV_YEAR = CALCULATE([Total Profit], DATEADD('Date'[Date].[Date], -1, YEAR))
	  RETURN
		  DIVIDE([Total Sales] - __PREV_YEAR, __PREV_YEAR)
    )
3. Profit_Icon = 
    Var PositiveIcon = UNICHAR(9650)
    Var NegativeIcon = UNICHAR(9660)
    Var Result = IF([Profit YoY%] > 0, PositiveIcon, NegativeIcon)

    RETURN Result
4. Profit_Icon_Color = IF([Profit YoY%] > 0, "Green","Red")
5. Sales LY = CALCULATE([Total Sales], DATEADD('Date'[Date], -1, YEAR))
6. Sales YoY% = 
    IF(
	    ISFILTERED('Date'[Date]),
	    ERROR("Time intelligence quick measures can only be grouped or filtered by the Power BI-provided date hierarchy or primary date column."),
	    VAR __PREV_YEAR = CALCULATE([Total Sales], DATEADD('Date'[Date].[Date], -1, YEAR))
	  RETURN
		  DIVIDE([Total Sales] - __PREV_YEAR, __PREV_YEAR)
    )
7. Sales_Icon = 
    Var PositiveIcon = UNICHAR(9650)
    Var NegativeIcon = UNICHAR(9660)
    Var Result = IF([Sales YoY%] > 0, PositiveIcon, NegativeIcon)

  RETURN Result
8. Sales_Icon_Color = IF([Sales YoY%] > 0, "Green","Red")
9. Total Profit = SUM(Sales_Data[Sales]) - SUM(Sales_Data[Cost])
10. Total Sales = 
      CALCULATE(
	      SUM('Sales_Data'[Sales]),
	      FILTER(
		    ALLSELECTED('Sales_Data'[Sales]),
		    ISONORAFTER('Sales_Data'[Sales], MAX('Sales_Data'[Sales]), DESC)
	      )
      )
11. Date = CALENDAR(MIN(Sales_Data[Order Date]), MAX(Sales_Data[Order Date]))

[NOTE: Sr. No. 1 to 10 are new measures under 'Sales_Data" table and Sr. No. 11 is a new table.]

- Create Visuals:
  
    a. Total Sales -
    
        Card
        
        Fields = Total Sales
      
    b. Total Profit -
    
        Card
        
        Fields = Total Profit
        
    c. Total Order Quantity -
    
        Card
        
        Fields = Total Order Quantity
        
    d. Total Products -
    
        Card
        
        Fields = Total Products
        
    e. Monthly Sales Trend -
    
        Area chart
        
        X-axis = Date
        
        Y-axis = Total Sales
        
    f. Weekly Sales Trend -
    
        Stacked Column chart
        
        X-axis = Week
        
        Y-axis = Total Sales
        
    g. Daily Sales Trend -
    
        Line chart
        
        X-axis = Day
        
        Y-axis = Total Sales
        
    h. Total Sales by Quarter -
    
        Stacked Bar chart
        
        X-axis = Quarter
        
        Y-axis = Total Sales
        
    i. Sales by Brand -
    
        Stacked Bar chart
        
        Y-axis = Brand Name

        X-axis = Total Sales
        
    j. Sales by Category -
    
        Stacked Column chart
        
        X-axis = Product Category

        Y-axis = Total Sales

    k. Sales by Channel -

        Donut chart

        Legend = ChannelName

        Values = Total Sales

    l. Profit by Channel -

        Pie chart

        Legend = ChannelName

        Values = Total Profit
  
    m. Top-10 Products -

        Columns = ProductName, Sum of Order Qty, Total Sales, Total Profit

        From Filter pane, filter the ProductName using Top N based on Total Sales.

    n. Sales by State -

        Map

        Location = State

    o. Total Sales and Profit by Zones -

        Line and Stacked column chart

        X-axis = Zone

        Column Y-axis = Total Sales

        Line Y-axis = Total Profit

    p. Sales by Promotion Name -

        Stacked Bar chart

        Y-axis = PromotionName

        X-axis = Total Sales

     q. Sales by Discount Mode -

        Donut chart

        Legend = Promotion Type

        Values = Total Sales

     r. Profit by Discount Mode -

        Pie chart

        Legend = Promotion Type

        Values = Total Profit

     s. Forecasting

  
### Key Metrics and Visualizations:
  
  -The total sales for the current year (CY) and the previous year (PY):
	Current Year: 23.80M (39.3% increase from PY's 17.09M)
	Previous Year: 21.94M (51% increase from PY's 15.73M)
  
  -Total number of orders placed is 251K.

  -Total number of unique products sold, totaling 1690
  
  -Fabrikam: Leading with the highest sales, around 4 million.
   Contoso: Second highest, close to Fabrikam's sales.
   Adventure Works: Third, with significant sales.
   Wide World Importers, Proseware, The Phone Company, A. Datum, Southridge Video, Litware, Northwind Traders: Each with progressively lower sales.
  
  -Computers leading sales category with sales of $9.1 million.
  
  -Store is the major sales channel with 57.43% of total sales contributing 56.69% of total profit and Mobile Outlet is the smallest sales channel with 8.84% with 9.09% of total profit.
  
  -States with significant sales include:
	Lagos
	Kano
	Kaduna
	Osun
	Niger
  
  -North West is the leading zone with sales over $4 million and the highest profit.


### Usage:
This dashboard is designed for stakeholders and enthusiasts in the retail and sales industries, including sales managers, marketing teams, and business analysts. It provides a comprehensive view of sales performance and key metrics to help identify trends, evaluate promotional effectiveness, and understand regional and product-specific sales dynamics.

### Dataset:
https://docs.google.com/spreadsheets/d/1RF2UG0zJ_BrKR7CqoSHaHxroSHvXQX2c/edit?usp=sharing&ouid=112148025722696529507&rtpof=true&sd=true
