# Challenge Set 9
## Part I: W3Schools SQL Lab 

*Introductory level SQL*

--

This challenge uses the [W3Schools SQL playground](http://www.w3schools.com/sql/trysql.asp?filename=trysql_select_all). Please add solutions to this markdown file and submit.

1. Which customers are from the UK?
```
SELECT CustomerName FROM Customers WHERE Country='UK';
```
CustomerName
Around the Horn
B's Beverages
Consolidated Holdings
Eastern Connection
Island Trading
North/South
Seven Seas Imports


2. What is the name of the customer who has the most orders?
```
SELECT CustomerName
FROM Customers
JOIN (SELECT CustomerID, COUNT(*) AS OrderCount 
FROM Orders 
GROUP BY CustomerID 
ORDER BY OrderCount DESC 
LIMIT 1) o
ON Customers.CustomerID = o.CustomerID;
```
CustomerName
Ernst Handel

3. Which supplier has the highest average product price?
```
SELECT SupplierName
FROM Suppliers
JOIN (SELECT SupplierID, AVG(Price) AS AvgPrice
FROM Products
GROUP BY SupplierID
ORDER BY AvgPrice DESC
LIMIT 1) p
ON p.SupplierID = Suppliers.SupplierID
```
SupplierName
Aux joyeux ecclésiastiques

4. How many different countries are all the customers from? (*Hint:* consider [DISTINCT](http://www.w3schools.com/sql/sql_distinct.asp).)
```
SELECT COUNT(DISTINCT Country) AS UniqueCountries
FROM Customers;
```
UniqueCountries
21


5. What category appears in the most orders?
```
SELECT CategoryID, COUNT(*) AS CategoryCount
FROM Products
JOIN OrderDetails
ON Products.ProductID = OrderDetails.ProductID
GROUP BY CategoryID
ORDER BY CategoryCount DESC
LIMIT 1;
```
CategoryID	CategoryCount
4	100

6. What was the total cost for each order?
```
SELECT OrderID, SUM(Price) AS Cost
FROM OrderDetails
JOIN Products
ON Products.ProductID = OrderDetails.ProductID
GROUP BY OrderID;
```
OrderID	Cost
10248	69.8
10249	76.25
10250	83.7
10251	61.55
10252	117.5
10253	50.5
10254	38.5
10255	110.45
10256	45.8
10257	74.9

7. Which employee made the most sales (by total price)?
```
SELECT LastName, FirstName, SUM(Quantity*Price) AS Revenue
FROM Orders
JOIN OrderDetails ON Orders.OrderID = OrderDetails.OrderID
JOIN Employees ON Employees.EmployeeID = Orders.EmployeeID
JOIN Products ON OrderDetails.ProductID = Products.ProductID
GROUP BY Employees.EmployeeID
ORDER BY Revenue DESC
LIMIT 1;
```
LastName	FirstName	Revenue
Peacock	Margaret	105696.49999999999


8. Which employees have BS degrees? (*Hint:* look at the [LIKE](http://www.w3schools.com/sql/sql_like.asp) operator.)
```
SELECT LastName, FirstName
FROM Employees
WHERE Notes LIKE '%BS%';
```
LastName	FirstName
Leverling	Janet
Buchanan	Steven

9. Which supplier of three or more products has the highest average product price? (*Hint:* look at the [HAVING](http://www.w3schools.com/sql/sql_having.asp) operator.)
```
SELECT SupplierName
FROM Suppliers
JOIN (SELECT SupplierID, AVG(Price) AS AvgPrice, COUNT(ProductID) AS ProductCount
FROM Products
GROUP BY SupplierID
HAVING ProductCount >= 3
ORDER BY AvgPrice DESC
LIMIT 1) p
ON p.SupplierID = Suppliers.SupplierID
```
SupplierName
Tokyo Traders
