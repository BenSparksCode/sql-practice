# SQL Notes

A place to store my SQL notes and practice problems. 

## Query Clause Order

1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. HAVING
6. ORDER BY

## [Basic SQL Concepts](https://mode.com/sql-tutorial/introduction-to-sql/)



### SELECT
```
SELECT year, month, west
FROM tutorial.us_housing_units
```

### LIMIT, AS
```
SELECT year AS "year_built", month, west
FROM tutorial.us_housing_units
LIMIT 20
```

### WHERE
```
SELECT *
FROM tutorial.us_housing_units
WHERE month_name = 'February'
```

```
SELECT *
FROM tutorial.us_housing_units
WHERE month_name < 'o'
```
This finds all month names that start with N or an earlier letter in alphabet. 

### Arithmetic
```
SELECT year, month, west, south,
  (west + south) / 2 AS south_west_avg
FROM tutorial.us_housing_units
```
This find the average across the south and west columns, per entry.

### Logical Operators 

- LIKE allows you to match similar values, instead of exact values.
- ILIKE is the same as LIKE but ignores case
- IN allows you to specify a list of values you'd like to include.
- BETWEEN allows you to select only rows within a certain range.
- IS NULL allows you to select rows that contain no data in a given column.
- AND allows you to select only rows that satisfy two conditions.
- OR allows you to select rows that satisfy either of two conditions.
- NOT allows you to select rows that do not match a certain condition.

### Wild Cards (for string matching)

- % = Any set of characters
- _ = A single character

### IN Operator

IN checks if the entry's column value is included in a given set of values.

```
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE artist IN ('Taylor Swift', 'Usher', 'Ludacris')
```

### BETWEEN Operator

BETWEEN matches values that fall within a specified range.

```
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE year_rank BETWEEN 5 AND 10
```

### ORDER BY

Sorts rows by a specific column(s). Sorts from smallest to largest by default (Ascending).

Use the DESC keyword after the column name to sort from largest to smallest instead.

```
SELECT *
FROM tutorial.billboard_top_100_year_end
WHERE year_rank <= 3
ORDER BY year DESC, year_rank
```

### Comments in SQL
```
--This is a single line comment in SQL


/* Here's

a multi-line

comment in SQL */
```

## [Intermediate SQL Concepts](https://mode.com/sql-tutorial/intro-to-intermediate-sql/)

Aggregation functions return a single row with one or many aggregated columns.

### COUNT

Place on the SELECT line. Can be used on non-numerical columns.

- SELECT COUNT(*) = count all results
- SELECT COUNT(price_high) = only count non-null values in that column

```
SELECT COUNT(date)
FROM tutorial.aapl_historical_stock_price
```

### SUM

Only works on numerical (summable) columns. Nulls will be evaluated as 0 in the SUM.

```
SELECT SUM(volume)
FROM tutorial.aapl_historical_stock_price
```

### MIN / MAX

Pulls the smallest/biggest value from the column.

```
SELECT MIN(volume) AS min_volume,
       MAX(volume) AS max_volume
FROM tutorial.aapl_historical_stock_price
```

### AVG

AVG finds an arithmetic mean average of a column.

Only accepts numerical columns and completely ignores null values. To include nulls as 0, pre-convert them to 0 before aggregating with AVG.

```
SELECT AVG(high)
FROM tutorial.aapl_historical_stock_price
WHERE high IS NOT NULL
```

### GROUP BY

Aggregation functions like COUNT, AVG, and SUM aggregate across entire table (result in 1 row)

GROUP BY can aggregate by parts of the table resulting in multiple rows (aggregated on sub-categories e.g. month of the year)

GROUP BY comes after FROM in the query.

```
SELECT year, COUNT(*) AS count
FROM tutorial.aapl_historical_stock_price
GROUP BY year
```
or GROUP BY multiple columns, which usually also needs an ORDER BY to order the results in a sensible way:

```
SELECT year,
       month,
       COUNT(*) AS count
FROM tutorial.aapl_historical_stock_price
GROUP BY year, month
ORDER BY month, year
```

### HAVING

HAVING is similar to the WHERE clause, but useful when you can't use the WHERE clause (when aggregating with GROUP BY).

```
SELECT year,
       month,
       MAX(high) AS month_high
FROM tutorial.aapl_historical_stock_price
GROUP BY year, month
HAVING MAX(high) > 400
ORDER BY year, month
```

### CASE

CASE allows handling of if/then logic.

Must be followed by at least one WHEN/THEN statement.

- WHEN specifies criteria
- THEN specifies the value result of that criteria
- Finish with an ELSE case and then END AS [new_col_name]

This query creates the 'is_a_senior' column, and fills the data in dependent on if that entry has 'SR' as the value in the 'year' column.
```
SELECT player_name,
       year,
       CASE WHEN year = 'SR' THEN 'yes'
            ELSE 'no' END AS is_a_senior
FROM benn.college_football_players
```
A more complex example:
```
SELECT player_name,
       weight,
       CASE WHEN weight > 250 THEN 'over 250'
            WHEN weight > 200 AND weight <= 250 THEN '201-250'
            WHEN weight > 175 AND weight <= 200 THEN '176-200'
            ELSE '175 or under' END AS weight_group
FROM benn.college_football_players
```
CASE with aggregate functions:
```
SELECT CASE WHEN year = 'FR' THEN 'FR'
            WHEN year = 'SO' THEN 'SO'
            WHEN year = 'JR' THEN 'JR'
            WHEN year = 'SR' THEN 'SR'
            ELSE 'No Year Data' END AS year_group,
            COUNT(1) AS count
FROM benn.college_football_players
GROUP BY CASE WHEN year = 'FR' THEN 'FR'
               WHEN year = 'SO' THEN 'SO'
               WHEN year = 'JR' THEN 'JR'
               WHEN year = 'SR' THEN 'SR'
               ELSE 'No Year Data' END
```
CASE inside aggregate functions:
```
SELECT COUNT(CASE WHEN year = 'FR' THEN 1 ELSE NULL END) AS fr_count,
       COUNT(CASE WHEN year = 'SO' THEN 1 ELSE NULL END) AS so_count,
       COUNT(CASE WHEN year = 'JR' THEN 1 ELSE NULL END) AS jr_count,
       COUNT(CASE WHEN year = 'SR' THEN 1 ELSE NULL END) AS sr_count
FROM benn.college_football_players
```

### DISTINCT

Returns only unique values.

In aggregations, DISTINCT goes inside the aggregation function.

DISTINCT can cause very slow queries in aggregations.

```
SELECT COUNT(DISTINCT month) AS unique_months
FROM tutorial.aapl_historical_stock_price
```

### Basic JOIN

Combines data from multiple tables, by connecting them via some relationship.

Follows this pattern:
- FROM [table1]
- JOIN [table2]
- ON [column in table1] = [column in table2]

```
SELECT *
FROM benn.college_football_players players
JOIN benn.college_football_teams teams
ON teams.school_name = players.school_name
```

### INNER JOIN

Equivalent of intersection between 2 sets (tables).

```
SELECT players.*, teams.*
FROM benn.college_football_players players
JOIN benn.college_football_teams teams
ON teams.school_name = players.school_name
```

### OUTER JOIN

Similar to INNER JOIN but OUTER JOIN can return unmatched rows in one or both tables.

Types of outer join:
- LEFT JOIN = Returns only unmatched rows from left table.
- RIGHT JOIN = Returns only unmatched rows from right table.
- FULL OUTER JOIN = Returns unmatched rows from both tables. 

### LEFT JOIN

Returns unmatched and matched rows from the left table, and only matched rows from the right table.

```
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
```

### RIGHT JOIN

Returns unmatched and matched rows from the right table, and only matched rows from the left table.

```
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
FROM tutorial.crunchbase_acquisitions acquisitions
RIGHT JOIN tutorial.crunchbase_companies companies
ON companies.permalink = acquisitions.company_permalink
```

### WHERE vs ON in JOINs

Filtering with ON will apply only in one table (before joining):

```
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
AND acquisitions.company_permalink != '/company/1000memories'
ORDER BY 1
```

Filtering with WHERE is applied after the data join, and so applies to data from both tables:

```
SELECT companies.permalink AS companies_permalink,
       companies.name AS companies_name,
       acquisitions.company_permalink AS acquisitions_permalink,
       acquisitions.acquired_at AS acquired_date
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_acquisitions acquisitions
ON companies.permalink = acquisitions.company_permalink
WHERE acquisitions.company_permalink != '/company/1000memories'
OR acquisitions.company_permalink IS NULL
ORDER BY 1
```

### UNION

UNION lets you write multiple different SELECT queries, then combine them into a single table of results.

UNION only selects distinct values by default, but UNION ALL will avoid this and include duplicates. 

```
SELECT *
FROM tutorial.crunchbase_investments_part1

UNION

SELECT *
FROM tutorial.crunchbase_investments_part2
```

### JOINs with Comparison Operators

Applies a fitlering step to a table in the JOIN process (so before data from other tables is involved).

```
SELECT companies.permalink,
       companies.name,
       companies.status,
       COUNT(investments.investor_permalink) AS investors
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_investments_part1 investments
ON companies.permalink = investments.company_permalink
WHERE investments.funded_year > companies.founded_year + 5
GROUP BY 1,2, 3
```

### JOINs on Multiple Keys

JOINing on multiple fields is useful for:
1. Accuracy (refines the results further)
2. Performance (SQL Indexes can be used to create faster queries even if no further accuracy is achieved)

```
SELECT companies.permalink,
       companies.name,
       investments.company_name,
       investments.company_permalink
FROM tutorial.crunchbase_companies companies
LEFT JOIN tutorial.crunchbase_investments_part1 investments
ON companies.permalink = investments.company_permalink
AND companies.name = investments.company_name
```

### Self JOINs

Sometimes useful to join a table with itself.

E.g. Finding companies that received investment from England after an investment from Japan:

```
SELECT DISTINCT japan_investments.company_name,
       japan_investments.company_permalink
FROM tutorial.crunchbase_investments_part1 japan_investments
JOIN tutorial.crunchbase_investments_part1 gb_investments
ON japan_investments.company_name = gb_investments.company_name
AND gb_investments.investor_country_code = 'GBR'
AND gb_investments.funded_at > japan_investments.funded_at
WHERE japan_investments.investor_country_code = 'JPN'
ORDER BY 1
```

## [Advanced SQL Concepts](https://mode.com/sql-tutorial/intro-to-advanced-sql/)

### Data Types

- String = VARACHAR(1024) = Any characters, max length 1024 chars
- Date/Time = TIMESTAMP = Date and time value
- Number = DOUBLE PRECISION = Up to 17 digits, with decimals
- Boolean = BOOLEAN = True/False

Can use CAST or CONVERT to change data types in queries.

```
SELECT CAST(funding_total_usd AS varchar) AS funding_total_usd_string,
       founded_at_clean::varchar AS founded_at_string
FROM tutorial.crunchbase_companies_clean_date
```

### String Functions

- LEFT(s,n), RIGHT(s,n) = selects substring of n chars starting from left/right of string s. 
- LENGTH(s) = Returns the length of string s.
- TRIM(both '()' FROM col) = Removes all brackets from start and end of the string in col. 
- POSITION('c' in name) = Returns the numerical position of character 'c' in the name value.
- STRPOS(name, 'c') = Same thing as POSITION
- SUBSTR(name, 4, 2) = Returns substring in name starting 4 chars in, with a length of 2 chars.
- CONCAT(name, ',' surname) = Returns a joined string, separated by a comma in this case.
- || = Same as CONCAT, works like '+' between strings in Python.
- UPPER/LOWER = Converts string to upper/lower case.
- EXTRACT('year' FROM date) = Returns the year part of the date.
- COALESCE(name, 'no name') = Replaces null values in a column with a specified value.  