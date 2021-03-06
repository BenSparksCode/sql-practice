# 176. Second Highest Salary

https://leetcode.com/problems/second-highest-salary/

Wrapping outer SELECT needed to include a 'null' value if there is no 2nd highest salary. Otherwise no rows returned in that case. 

### Solution
```
SELECT (
    SELECT DISTINCT Salary 
    FROM Employee
    ORDER BY Salary DESC
    LIMIT 1 OFFSET 1) AS SecondHighestSalary 
```

### Results
- Runtime: 164 ms, faster than 93.13% of MySQL online submissions for Second Highest Salary.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Second Highest Salary.
