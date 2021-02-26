# SQL Practice
A place for my SQL notes and practice problem solutions.


# SQL Notes

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
SELECT
  year,
  month,
  west,
  south,
  (west + south) / 2 AS south_west_avg
FROM
  tutorial.us_housing_units
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
SELECT
  *
FROM
  tutorial.billboard_top_100_year_end
WHERE
  artist IN ('Taylor Swift', 'Usher', 'Ludacris')
```

### Aggregation

```

```

## Advanced SQL Concepts

```

```