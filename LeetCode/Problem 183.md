# 183. Customers Who Never Order

https://leetcode.com/problems/customers-who-never-order/

Nested query to pull all CustomerIds that appear in the Orders table, then just filter by WHERE Id NOT IN that set of Ids.

### Solution
```
SELECT Name AS Customers
FROM Customers
WHERE Id NOT IN (
    SELECT CustomerId
    FROM Orders
) 
```

### Results
- Runtime: 525 ms, faster than 23.34% of MySQL online submissions for Customers Who Never Order.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Customers Who Never Order.