SQL NOTEBOOK

I will use the Northwind data set, you can find it here: https://github.com/jpwhite3/northwind-SQLite3?tab=readme-ov-file
The data set contains 13 Tables . I will be using SQLite v 3.7
SQL’s logical order is: FROM/JOIN → WHERE → GROUP BY → HAVING → SELECT → ORDER BY


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

8. DISTINCT - removes duplication results 
9. ORDER BY - arrange the results by the column you choose. ASC or DESC. Happens last
10. IS NULL / IS NOT NULL - filters the results to a NULL or dont show NUlls
11. CAST - change the expressions data type - e.g change string data to INT . common conversion: string (VARCHAR), intreger (INT), decimal (numeric) , DATE (DATE)
    CAST(expression AS data_type) 
13. DATEDIFF -  calculates the difference between 2 provided columns date. DATEDIFF(day or month or year, column1,column2) = bring me the delta between column 1 and column 2
14. DATETRUNK - Giving a period of time, sql will see all of the orders as the  start of the same timeline. DATETRUNK('MONTH' or 'DAY or 'YEAR' or 'MONTH', column1)

  ### AGRREGATIONS:
    Aggregation is a formulated calculation + compression made on column rows.
16. COUNT(DISTINCT column) -counts the number of times different value is in a column row without duplications and NULL
17. COUNT (*) - Returns the total number of rows in a table or the results set, including duplicate rows and NULL. [number of rows in each group (or entire result if no GROUP BY)]
18. COUNT(COLUMN)- counts the number of times the value is in the column rows return it (without NULLS) 
19. SUM - SUMMARISE the values
20. AVG - calculates the AVERAGE of the column/ columns
21. GROUP BY - collapses the rows into a bucket with the same  identifier, e.g GROUP BY (month) will collapse the rows of days within the same column month to a single month
22. HAVING - adds a condition to the aggregation , just like the where caluse but for aggregated rows
23. MIN / MAX (COLUMN) - show me the min or max of the values in the column

  ### SUBQUERIES ,CTE. WINDOW FUNCTION
24.SUBQUERIES - is a simple select query inside a select statement. you use it when you need : a filtered list, temporary result , a value calculated first. 
25.CTE-CTE (Common Table Expression) CTE is just a named subquery
26 WINDOW FUNCTIONS - are used for comparing rows (aggregated and non-aggregated) 
