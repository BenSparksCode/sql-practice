# 178. Rank Scores

https://leetcode.com/problems/rank-scores/

Selecting from normal Scores table (S) as well as a nested query (S2) that filters Scores by only taking DISTINCT values (so same scores will get same Rank).

Then for each Score in S, COUNT how many DISTINCT scores appear in S2 that are <= that S.Score. This is the rank of S.Score.

Finally, group those results by Id so that we show duplicate scores as the same rank. ORDER BY Score DESC to show highest scores at top. 

### Solution
```
SELECT S.Score, COUNT(S2.Score) AS `Rank`
FROM Scores S, (
    SELECT DISTINCT Score
    FROM Scores
) S2
WHERE S.Score <= S2.Score
GROUP BY S.Id
ORDER BY Score DESC
```

### Results
- Runtime: 490 ms, faster than 16.90% of MySQL online submissions for Rank Scores.
- Memory Usage: 0B, less than 100.00% of MySQL online submissions for Rank Scores.