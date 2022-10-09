# PostgreSQL
Perform an operation across a set of rows that are somehow related to the current row.

### Numeric Data Type
<img width="881" alt="image" src="https://user-images.githubusercontent.com/92245436/194701074-47099a83-e365-4623-88e2-61e376713786.png">

## Window Function
### Ranking
- `ROW_NUMBER()` - always assigns unique numbers, even if two rows' values are the same
- `RANK()` - assigns the same number to rows with identical values, skipping over the next numbers in such cases
- `DENSE_RANK()` -  also assigns the same number to rows with identical values, but doesn't skip over the next numbers
```
SELECT Country, Games,
       ROW_NUMBER() OVER (ORDER BY Games DESC) AS Row_N,
       RANK() OVER (ORDER BY Games DESC) AS Rank_N,
       DENSE_RANK() OVER (ORDER BY Games DESC) AS Dense_Rank_N
FROM Country_Games
ORDER BY Games DESC, Country ASC;
```
- ` PARTITION BY` - splits the table into partitions based on a column's unique values.    


#### Relative
- `LAG(column, n)` returns column's value at the row n rows before the current row
- `LEAD(column, n)` returns column 's value at the row n rows aer the current row
#### Absolute
- `FIRST_VALUE(column)` returns the rst value in the table or partition
- `LAST_VALUE(column)` returns the last value in the table or partition


```
SELECT
  Gender, Year, Event,
  Country AS Champion,
  -- Fetch the previous year's champion by gender and event
  LAG(Country) OVER (PARTITION BY GENDER,EVENT ORDER BY Year ASC) AS Last_Champion
FROM Athletics_Gold
ORDER BY Event ASC, Gender ASC, Year ASC;
```
#### Paging - Spliing data into (approximately) equal chunks
- `NTILE(n)`- splits the data into n approximately equal pages
```
SELECT
Discipline, NTILE(15) OVER () AS Page
From Disciplines
ORDER BY Page ASC;
```

## Summarize Functions
- min(), avg(), max(), and stddev()

```
SUM(medals) OVER (ORDER BY Athlete ASC) AS Max_Medals
MAX(Medals) OVER (PARTITION BY Country ORDER BY Year ASC) AS Max_Medals
MIN(Medals) OVER (ORDER BY Year ASC) AS Min_Medals
```
### Frames
### ROWS BETWEEN
ROWS BETWEEN [START] AND [FINISH]
- `n PRECEDING` : n rows before the current row
- `CURRENT ROW` : the current row
- `n FOLLOWING` : n rows aer the current row
```
ROWS BETWEEN 3 PRECEDING AND CURRENT ROW
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING
ROWS BETWEEN 5 PRECEDING AND 1 PRECEDING
```
#### Moving Average
```
SELECT
  Year, Medals,
  --- Calculate the 3-year moving average of medals earned
  AVG(Medals) OVER (ORDER BY Year ASC ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS Medals_MA
FROM Russian_Medals
ORDER BY Year ASC;
```
|year|	medals|medals_mov_avg|
|-----|-------|-----|
|1996	|36|	36.00|
|2000	|66|	51.00|
|2004|	47|	49.66|

`coalesce()` -  checks arguments in order and returns the first non-NULL value, if one exists.
```
coalesce(NULL, 1, 2) = 1
```
`trunc()` -  truncates numbers by replacing lower place value digits with zeros.
```
trunc(value_to_truncate, places_to_truncate)
```
### Chnage Type 
```
SELECT CAST(value AS new_type);

SELECT value::new_type;
```





#### Generate series
`generate_series(from, to, step)`
```
SELECT generate_series(2900 , 3050, 50 ) AS lower,
       generate_series(2950, 3100, 50) AS upper;
```

|lower	|upper|
|-----|------|
|2900	|2950|
|2950	|3000|
|3000	|3050|
|3050|	3100|

#### Correlation
`corr( ColA,ColB)`

####  Percentile
```
percentile_disc(0.5) 
WITHIN GROUP (ORDER BY column_name)
```
### CreateTempTableSyntax
```
-- Create table as
CREATE TEMP TABLE new_tablename AS
-- Query results to store in the table
SELECT column1, column2
FROM table;
```
```
-- Select existing columns
SELECT column1, column2 
-- Clause to direct results to a new temp table
INTO TEMP TABLE new_tablename
-- Existing table with exisitng columns
FROM table;
```
#### Trim 
trim(street, '0123456789 #/.')

- split_part
`split_part(string_to_split, delimiter, part_number)`
