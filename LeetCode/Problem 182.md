# 182. Duplicate Emails

https://leetcode.com/problems/duplicate-emails/

Use a nested SELECT query to pull unique emails (using GROUP BY) and the amount of times they appear (using COUNT).

Then from that query, SELECT Emails and filter to emails that have a count greater than 1. 

### Solution
```
SELECT Email
FROM (
    SELECT Email, COUNT(Email) AS Cnt
    FROM Person
    GROUP BY Email
) p
WHERE Cnt > 1
```

OR 

```
SELECT Email
FROM Person
GROUP BY Email
HAVING COUNT(Email) > 1;
```

### Results
- Runtime: 275 ms, faster than 89.78% of MySQL online submissions for Duplicate Emails.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Duplicate Emails.