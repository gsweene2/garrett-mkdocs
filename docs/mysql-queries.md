# MySql Queries

A random collection of queries from the [w3schools sql tutorial](https://www.w3schools.com/sql/default.asp).

Return a distinct list of Countries from the Customer table.
```sql
SELECT DISTINCT Country 
FROM Customers 
ORDER BY Country;
```

Return the Products where the price is less than $40 in descending order. Return only 5 Products.
```sql
SELECT * 
FROM Products 
WHERE Price < 40
ORDER BY ProductName DESC
LIMIT 5;
```

Count the number of distinct countries in the Customer table
```sql
SELECT Count(*) AS TOTAL_RECORDS_OF_UNIQUE_COUNTRY
FROM (SELECT DISTINCT Country FROM Customers);
```

Return the Customers where Customer ID is 1, 5, or 8.
```sql
SELECT * 
FROM Customers 
WHERE CustomerID IN (1,5,8);
```

Return the Orders that took place between August 8th, 1996 and August 11th, 1996.
```sql
SELECT * FROM [Orders]
WHERE OrderDate BETWEEN "1996-07-08" AND "1996-07-11";
```

Return the Customers that are from Germany but are from a City that does not start with B and the Postal Code ends in 5.
```sql
SELECT * 
FROM Customers
WHERE 
	Country='Germany' AND 
    City NOT LIKE 'B%' AND
    PostalCode LIKE '%5';
```
