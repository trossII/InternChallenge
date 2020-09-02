# InternChallenge

Winter 2021 Data Science Intern Challenge 

Please complete the following questions, and provide your thought process/work. You can attach your work in a text file, link, etc. on the application page. Please ensure answers are easily visible for reviewers!


Question 1: Given some sample data, write a program to answer the following: click here to access the required data set

On Shopify, we have exactly 100 sneaker shops, and each of these shops sells only one model of shoe. We want to do some analysis of the average order value (AOV). When we look at orders data over a 30 day window, we naively calculate an AOV of 3145.13 dollars. Given that we know these shops are selling sneakers, a relatively affordable item, something seems wrong with our analysis. 

Think about what could be going wrong with our calculation. Think about a better way to evaluate this data. 

Hearing this calculation the first thing I want to do is get this data imported and take a look at the distribution of the orders. So I used the .describe function in Pandas and I can immediately see that this dataset has a Mean of  3,145.128 (the value calculated previously) and a Standard Deviation of 41282.54, telling me that this data is very skewed and for a better representation of the data we should be using the median instead. So by using the median order value (MOV) we get a value of 284 dollars which is much more reasonable.

Now if we want to go deeper down the rabbit hole, we can look at some other aspects of the order_amount column such as the max we can determine that there are orders up to 704,000 dollars which is the cause of the skew in our data if we are looking for an amount from our average purchaser. Looking further into these 704,000 dollar purchases we can see that these transactions are between the same buyer and seller, and that these are the only purchases from this buyer. So after reporting this weird transactions to the accounting team so they can look further into the potential issues of some kind of fraud (especially since there seems to be instances of multiple of the same transaction going through on the same date at the same time), I think it would be reasonable to remove this buyer from the dataset and recalculate the AOV. This gives us an AOV of 754.09 dollars but we still have a Standard Deviation of 5314.09, so I would still recommend using the median. 

The data is still skewed due to company 78 selling a shoe for around $25,000, though their transactions still seem legit and do not throw any flags to me with dates/time, I would suggest making further inquiries since this company is such an outlier on the charts I created.


What metric would you report for this dataset?
Median Order Value

What is its value?
$284


Question 2: For this question you’ll need to use SQL. Follow this link to access the data set required for the challenge. Please use queries to answer the following questions. Paste your queries along with your final numerical answers below.

How many orders were shipped by Speedy Express in total?
54

SELECT COUNT(OrderID) AS "Orders from Speedy Express" FROM Orders
WHERE ShipperID = 1;

Or if you want to look up by the name of the shipper instead of the ShipperID the following query would work:

SELECT COUNT(OrderID) AS "Number of Orders", ShipperName FROM Shippers
RIGHT JOIN Orders 
ON Orders.ShipperID = Shippers.ShipperID
GROUP BY ShipperName
HAVING ShipperName like "Speedy Express";

What is the last name of the employee with the most orders?
Peacock

SELECT TOP 1 LastName FROM Orders
LEFT JOIN Employees
ON Employees.EmployeeID = Orders.EmployeeID
GROUP BY Orders.EmployeeID, LastName
ORDER BY COUNT(Orders.EmployeeID) Desc;

What product was ordered the most by customers in Germany?
Steeleye Stout

SELECT TOP 1 P.ProductName
FROM ( ( ( Orders O INNER JOIN OrderDetails OD ON OD.OrderID = O.OrderID)
INNER JOIN Customers C ON C.CustomerID = O.CustomerID)
INNER JOIN Products P ON P.ProductID = OD.ProductID)
WHERE C.Country like "Germany"
GROUP BY P.ProductName,OD.Quantity

