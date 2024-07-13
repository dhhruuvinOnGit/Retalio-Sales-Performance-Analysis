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

    l. Profit by Channel

        Pie chart

        Legend = ChannelName

        Values = Total Profit
  
    m. Top-10 Products

        Columns = ProductName, Sum of Order Qty, Total Sales, Total Profit

        From Filter pane, filter the ProductName using Top N based on Total Sales.

    n. Sales by State

        Map

        Location = State

    o. Total Sales and Profit by Zones

        Line and Stacked column chart

        X-axis = Zone

        Column Y-axis = Total Sales

        Line Y-axis = Total Profit

    p. Sales by Promotion Name

        Stacked Bar chart

        Y-axis = PromotionName

        X-axis = Total Sales

  
### Key Metrics and Visualizations:
  
  -The total number of electric vehicles (EVs) in the dataset, with a figure of 150.48K.
  
  -Average electric range of the vehicles is 67.88 kms.
  
  -BEVs (Battery Electric Vehicles): 117K, which is 77.6% of the total EVs.
   PHEVs (Plug-in Hybrid Electric Vehicles): 34K, which is 22.4% of the total EVs.
  
  -A line chart that shows the growth of EVs from 2010 onwards. The chart highlights significant growth over the years, peaking at 37K in 2023.
  
  -A treemap visualization showcasing the top 10 EV models by total number of vehicles. Notable models include:
  Model Y: 28.50K
  Model 3: 27.71K
  LEAF: 13.19K
  
  -A bar chart indicating the top vehicle manufacturers by the number of EVs. Tesla leads with 69K vehicles, followed by Nissan with 13K and Chevrolet with 12K.
  
  -A pie chart that classifies vehicles based on CAFV eligibility:
  Unknown (Battery not researched): 17.83K (11.85%)
  Eligible: 62.95K (41.83%)
  Not Eligible: 69.7K (46.32%)
  
  -A map of the United States highlighting the distribution of EVs across different states, providing a geographical perspective on EV adoption.


### Usage:
This dashboard is designed for stakeholders and enthusiasts in the electric vehicle industry, including policymakers, manufacturers, and consumers. It can help identify trends, compare the performance of different EV models, and understand the impact of EVs in various regions.

### Dataset:
https://drive.google.com/file/d/13-V3vR2QWJKjV1wzIElVTqgziOZ-YrCn/view?usp=sharing
