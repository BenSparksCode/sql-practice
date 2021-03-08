# 180. Consecutive Numbers

https://leetcode.com/problems/consecutive-numbers/

Pull from 3 different versions of Logs to compare the Num value at 3 consecutive but different entries in the Logs table.

Check that the 3 entries compared are consecutive with 

```L1.Id = L2.Id-1 AND L2.Id = L3.Id-1```

Then check the numbers for these 3 entries are the same with

```L1.Num = L2.Num AND L2.Num = L3.Num```

Finally, use DISTINCT to pick one entry for each set of 3.

### Solution
```
SELECT DISTINCT L1.Num AS ConsecutiveNums
FROM Logs L1, Logs L2, Logs L3
WHERE
    L1.Id = L2.Id-1 AND
    L2.Id = L3.Id-1 AND
    L1.Num = L2.Num AND
    L2.Num = L3.Num
```

### Results
- Runtime: 457 ms, faster than 43.14% of MySQL online submissions for Consecutive Numbers.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Consecutive Numbers.