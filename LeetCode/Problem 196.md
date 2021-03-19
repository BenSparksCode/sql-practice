# 196. Delete Duplicate Emails

https://leetcode.com/problems/delete-duplicate-emails/


Solution 1: Joins table Person with itself, only selects duplicate Emails where Id is greater than the duplicate entry, and deletes those higher Id duplicates.

Solution 2: Uses nested SELECT queries to find the entries with the lowest Id, grouped by Email to prevent duplicates, and deletes any entries not in that set.

### Solution 1
```
DELETE p1
FROM Person p1, Person p2
WHERE p1.Email = p2.Email
    AND p1.Id > p2.Id
```

### Solution 2
```
DELETE
FROM Person
WHERE Id NOT IN (
    SELECT *
    FROM (
        SELECT MIN(Id)
        FROM Person
        GROUP BY Email
    ) as p
)
```

### Results - Solution 1
- Runtime: 1570 ms, faster than 74.03% of MySQL online submissions for Delete Duplicate Emails.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Delete Duplicate Emails.

### Results - Solution 2

- Runtime: 1264 ms, faster than 99.26% of MySQL online submissions for Delete Duplicate Emails.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Delete Duplicate Emails.