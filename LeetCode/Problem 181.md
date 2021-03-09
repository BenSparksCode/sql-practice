# 181. Employees Earning More Than Their Managers

https://leetcode.com/problems/employees-earning-more-than-their-managers/

Get 2 different copies of Employee table, as a,b. 

Filter a as the employee and b as the manager using:

```a.ManagerId = b.Id```

Filter for employees earning more than their managers with:

```a.Salary > b.Salary```

### Solution
```
SELECT a.Name as Employee
FROM Employee AS a, Employee AS b
WHERE
    a.ManagerId = b.Id AND
    a.Salary > b.Salary
```

### Results
- Runtime: 342 ms, faster than 49.19% of MySQL online submissions for Employees Earning More Than Their Managers.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Employees Earning More Than Their Managers.