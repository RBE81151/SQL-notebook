SQL NOTEBOOK

I will use the Northwind data set, you can find it here: https://github.com/jpwhite3/northwind-SQLite3?tab=readme-ov-file
The data set contains 13 Tables . I will be using SQLite v 3.7
SQL’s logical order is: FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
SQL logical order of execution (→)

CTE (WITH)
→ FROM (incl. JOIN ... ON, subqueries in FROM)
→ WHERE (row filtering, subqueries in WHERE)
→ GROUP BY
→ HAVING (group filtering, subqueries in HAVING)
→ SELECT (expressions, aggregates, subqueries in SELECT)
→ WINDOW functions (OVER(...))
→ DISTINCT
→ UNION / INTERSECT / EXCEPT
→ ORDER BY
→ LIMIT / OFFSET
Rows → Groups → Values → Order → Cut


1. SELECT - the columns you write here are the results SQL will show. when join is applied, sql execute join before the select, therefore, writing a select can be used for multiple Tables. 
3. FROM - which table does the select take the columns from
4. WHERE - apply a filter to the columns. does not work on aggregated columns. 
5. LEFT JOIN- add a table to the query, can be used for multiple tables.LEFT JOIN will keep data from the left table and pull matching data, will pull NULLS if the data dosent match . ON - ON decides how rows match between tables.
6. INNER JOIN- keeps only rows that exist in BOTH tables according to the ON condition.
7. ALIASSES - giving columns a nickname. Recommended to use when a table has a column with space
   e.g: 
   SELECT o.OrderID, o.OrderDate ## show me these columns  
FROM "Order Details" AS od ## from "Order Details" and name it od - due to the column space its mandatory to nickname this table. also, the from clause is the main table
LEFT JOIN Orders AS o 
ON od.OrderID=o.OrderID ## I used the table's identifier (orderid) to connect the tables. 

8. DISTINCT - removes duplicate results 
9. ORDER BY - arrange the results by the column you choose. ASC or DESC. Happens last
10. IS NULL / IS NOT NULL - filters the results to a NULL or dont show NUlls
11. CAST - change the expression's data type - e.g change string data to INT . common conversion: string (VARCHAR), intreger (INT), decimal (numeric) , DATE (DATE)
    CAST(expression AS data_type) 
13. DATEDIFF -  calculates the difference between 2 provided columns date. DATEDIFF(day or month or year, column1,column2) = bring me the delta between column 1 and column 2
14. DATETRUNK - Giving a period of time, SQL will see all of the orders as the  start of the same timeline. DATETRUNK('MONTH' or 'DAY or 'YEAR' or 'MONTH', column1)
15. MOTNH/YEAR/DAY (COLUMN)- expression that will extract the specific month from a date format as a number. e.g YEAR(order_date), MONTH(order_date). it will only live in the select clause


  ### AGRREGATIONS:
    Aggregation is a formulated calculation + compression made on column rows.
16. COUNT(DISTINCT column) -counts the number of times a different value is in a column row without duplication and NULL
17. COUNT (*) - Returns the total number of rows in a table or the results set, including duplicate rows and NULL. [number of rows in each group (or entire result if no GROUP BY)]
18. COUNT(COLUMN)- counts the number of times the value is in the column rows, returning it (without NULLS) 
19. SUM - SUMMARISE the values
20. AVG - calculates the AVERAGE of the column/ columns
21. GROUP BY - collapses the rows into a bucket with the same  identifier, e.g GROUP BY (month) will collapse the rows of days within the same column month to a single month
22. HAVING - adds a condition to the aggregation , just like the where caluse but for aggregated rows
23. MIN / MAX (COLUMN) - show me the min or max of the values in the column

  ### SUBQUERIES, CTE. WINDOW FUNCTION
24.SUBQUERIES - is a simple select query inside a select,from,where,HAVING OR another subquery . you use it when you need : a filtered list, a temporary result , a value calculated first. 
IN/NOT IN OPERATOR - used to determine if a value matches any value in a specific list of values or the results of a subquery
E.G:  SELECT employee_id, department
FROM employees
WHERE department IN ('Sales', 'Marketing');

25.CTE-CTE (Common Table Expression) CTE is just a named subquery. 
   CTE will start with WITH before usually a select statement, 
   e.g: WITH monthly_sales AS ( select month (order_date) as month, sum (amount) as total_sales from orders group by month(order_date) )
   select * from month_sales
   
26 WINDOW FUNCTIONS - are used for analyzing rows (aggregated and non-aggregated) . 
   Normal SQL have 2 modes: 1.ROW MODE (you see every row as is) and 2. GROUP MODE (rows get collapsed - aggregated) .
   WINDOW functions give a third mode: Look at multiple rows, but don't collapse them. That way you don't get to see each row while calculating things like running totals, previous value, rank,and  average. 
OVER - says dont collapse rows, just look across them and give me the results. 
() - look at all rows
   e.g:   here SQL think - For each row, calculate SUM(amount) using all rows
   SELECT
     order_id,
     amount,
SUM(amount) OVER () AS total_sales  ## summerise the amount column with OVER () says dont collapse the rows, just use them and give me the results in a new column name total_sales
   FROM orders;
   window phrases: 
OVER(PARTITION BY COLUMN) - means split data into group but still no collapsing, rows stay. PARTITION BY = GROUP BY without collapsing.
   This optional subclause divides the rows into groups (partitions) to which the window function is applied independently. If omitted, the entire result set is treated as a single partition.
SUM(COLUMN) OVER
AVG(COLUMN) OVER
COUNT(COLUMN) OVER
LAG(COLUMN) - Accesses data from a previous row . lag would usually come with OVER (ORDER BY ...). Without ORDER BY, SQL has no concept of previous or next.
LEAD(COLUMN)- Accesses data from a subsequent row. looks forward (next row)
RANK() - Assigns the same rank to identical values, skipping the next number (e.g., 1, 2, 2, 4). after RANK() there will be OVER (ORDER BY COLUMN DESC) . Without it, ranking is meaningless because SQL must know rank based on what
DENSE_RANK() - same as RANK() but will ignor the gap of ranig. (e.g. 1,2,2,3) 
ROW_NUMBER() -same as RANK() but, it will ignor ties completely. (e.g , 1,2,3,4). meaning, its numbering rows (no ranking logic)
ORDER BY: This optional subclause defines the logical order of rows within each partition, which is crucial for functions like RANK() or for calculating running totals.

1. Month-over-month change
sales - LAG(sales)
Means:
current month minus previous month
2. Percent change
(sales - LAG(sales)) / LAG(sales)

DENSE_RANK() OVER (ORDER BY sales DESC) AS dr
| name | sales | dr |
| ---- | ----- | -- |
| A    | 300   | 1  |
| B    | 200   | 2  |
| C    | 200   | 2  |
| D    | 100   | 3  |

 Task - from the Northwind data set

For each order, return:
OrderID
OrderDate
net_revenue per order
SUM(UnitPrice * Quantity * (1 - Discount))
order_rank_of_day
→ Rank orders within the same OrderDate by net_revenue
→ Highest revenue = rank 1
→ Ties should receive the same rank
→ Next rank should skip numbers if there are ties

SOLUTION:

WITH TEMP_TABLE AS  ## first created CTE to use for the aggregation, that way, i can use WINDOW function to show 2 separate rows 
( 
SELECT 
od.OrderID, o.OrderDate, 
SUM(od.UnitPrice*od.Quantity*(1-od.Discount)) as net_revenue 

FROM [Order Details] as od
LEFT JOIN Orders as o
ON od.OrderID=o.OrderID
GROUP BY o.OrderDate, od.OrderID
)
SELECT 
OrderID, OrderDate, net_revenue,
RANK()OVER(PARTITION BY date(OrderDate) ORDER BY net_revenue DESC  
) as RANK_net_revenue
FROM TEMP_TABLE
ORDER BY date(OrderDate),RANK_net_revenue, OrderID 

note: date() clause is neccere


