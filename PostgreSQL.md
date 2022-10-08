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

### Numeric Data Type
<img width="881" alt="image" src="https://user-images.githubusercontent.com/92245436/194701074-47099a83-e365-4623-88e2-61e376713786.png">

#### Summarize Functions
- min(), avg(), max(), and stddev()


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
