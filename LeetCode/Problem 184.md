# 184. Department Highest Salary

https://leetcode.com/problems/department-highest-salary/

Get max salary per dept. using sub SELECT query, then check if each line in the JOIN query matches a line in that sub query with DeptID and Salary, using:

```
WHERE (x, y) IN (SELECT...)
```

### Solution
```
SELECT d.Name AS Department, e.Name AS Employee, e.Salary
FROM Employee e
JOIN Department d ON e.DepartmentId = d.Id
WHERE (e.DepartmentId, e.Salary) IN (
    SELECT DepartmentId, MAX(Salary) AS Salary
    FROM Employee
    GROUP BY DepartmentId
)
```

### Results
- Runtime: 517 ms, faster than 78.05% of MySQL online submissions for Department Highest Salary.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Department Highest Salary.