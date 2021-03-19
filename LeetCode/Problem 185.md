# 185. Department Top Three Salaries

https://leetcode.com/problems/department-top-three-salaries/

Similar to Problem 184, except subquery isn't a MAX GROUP BY, but rather counts the number of salaries in the Dept that are higher than a given salary (from the outer query), and limits the result by only taking those that have a count of less than 3 for salaries greater than themselves in their dept.

### Solution
```
SELECT d.Name AS Department, e1.Name AS Employee, e1.Salary
FROM Employee e1
JOIN Department d ON e1.DepartmentId = d.Id
WHERE 3 > (
    SELECT COUNT(DISTINCT e2.Salary)
    FROM Employee e2
    WHERE e2.Salary > e1.Salary
        AND e1.DepartmentId = e2.DepartmentId
)
```

### Results
- Runtime: 1016 ms, faster than 30.76% of MySQL online submissions for Department Top Three Salaries.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Department Top Three Salaries.