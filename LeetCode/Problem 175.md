# 175. Combine Two Tables

https://leetcode.com/problems/combine-two-tables/

### Solution
```
SELECT Person.FirstName, Person.LastName, Address.City, Address.State
FROM Person
LEFT JOIN Address ON Person.PersonID = Address.PersonID
```

### Results

- Runtime: 264 ms, faster than 96.22% of MySQL online submissions for Combine Two Tables.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Combine Two Tables.