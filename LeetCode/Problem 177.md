# 177. Nth Highest Salary

https://leetcode.com/problems/nth-highest-salary/

DECLARE and SET extra INT M to N-1 at start of function, to then use that value as the offset figure in the LIMIT of the SELECT.

### Solution
```
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
DECLARE M INT;
SET M=N-1;
RETURN (
    SELECT DISTINCT Salary
    FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET M
);
END
```

### Results
- Runtime: 287 ms, faster than 76.31% of MySQL online submissions for Nth Highest Salary.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Nth Highest Salary.
