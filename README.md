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

### ARITHMETIC
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



### AGGREGATION

```

```

## Advanced SQL Concepts

```

```