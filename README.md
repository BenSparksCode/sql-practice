# SQL Notes

A place to store my SQL notes and practice problems. 

## Query Clause Order

1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. HAVING
6. ORDER BY

## Basic SQL Concepts

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

## Intermediate SQL Concepts

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

## Advanced SQL Concepts

```

```