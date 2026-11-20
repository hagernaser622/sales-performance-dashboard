# DAX measures

All **17 measures** live in a table called `DAX`. The model has one fact table (`Fact-Sales`), so most measures are simple aggregations, and the time measures use the `Dim-Date` table.

## Sales and profit

### Total Sales

Sales **excluding tax**. Used on page 2 (Sales & Profit Analysis) and as the denominator of the profit margin.

```DAX
Total Sales = SUM('Fact-Sales'[Total Excluding Tax])
```

### Gross Sales

Sales **including tax**. Used on page 1 (Executive Overview charts) and in the page 3 product table.

```DAX
Gross Sales = SUM('Fact-Sales'[Total Including Tax])
```

### Total Tax

```DAX
Total Tax = SUM('Fact-Sales'[Tax Amount])
```

### Total Profit

```DAX
Total Profit = SUM('Fact-Sales'[Profit])
```

### Profit Margin %

Profit as a share of sales excluding tax. `DIVIDE` returns 0 instead of an error when sales are zero.

```DAX
Profit Margin % = DIVIDE([Total Profit], [Total Sales], 0)
```

## Orders and counts

### Total Quantity

```DAX
Total Quantity = SUM('Fact-Sales'[Quantity])
```

### Total Invoices

Counts each invoice once (distinct invoice ID).

```DAX
Total Invoices = DISTINCTCOUNT('Fact-Sales'[WWI Invoice ID])
```

### Average Order Value

Sales (excl. tax) per invoice.

```DAX
Average Order Value = DIVIDE([Total Sales], [Total Invoices], 0)
```

### Total Customers

```DAX
Total Customers = DISTINCTCOUNT('Fact-Sales'[Customer Key])
```

### Total Salespeople'

```DAX
Total Salespeople' = DISTINCTCOUNT('Fact-Sales'[Salesperson Key])
```

### Total Products Sold

Number of distinct products that were sold.

```DAX
Total Products Sold = DISTINCTCOUNT('Fact-Sales'[Stock Item Key])
```

## Time intelligence

All of them are based on `[Total Sales]` and the `Dim-Date[Date]` column.

### Sales MTD

```DAX
Sales MTD = TOTALMTD([Total Sales], 'Dim-Date'[Date])
```

### Sales YTD

```DAX
Sales YTD = TOTALYTD([Total Sales], 'Dim-Date'[Date])
```

### Sales Previous Month

```DAX
Sales Previous Month = CALCULATE([Total Sales], PREVIOUSMONTH('Dim-Date'[Date]))
```

### Sales Previous Year

```DAX
Sales Previous Year = CALCULATE([Total Sales], SAMEPERIODLASTYEAR('Dim-Date'[Date]))
```

### Sales YoY Change

```DAX
Sales YoY Change = [Total Sales] - [Sales Previous Year]
```

### Total Sales YoY %

```DAX
Total Sales YoY % = DIVIDE([Sales YoY Change], [Sales Previous Year], 0)
```
