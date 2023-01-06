# SQL Window Functions

- **Partition by:** A subclause of the OVER clause. Similar to GROUP BY.
- **Over:** Typically precedes the partition by that signals what to “GROUP BY”.
- **Aggregates:** Aggregate functions that are used in window functions, too (e.g., sum, count, avg).
- **Row_number():** Ranking function where each row gets a different number.
- **Rank():** Ranking function where a row could get the same rank if they have the same value.
- **Dense_rank():** Ranking function similar to rank() but ranks are not skipped with ties.
- **Aliases:** Shorthand that can be used if there are several window functions in one query.
- **Percentiles:** Defines what percentile a value falls into over the entire table.
- **Lag/Lead**: Calculating differences between rows’ values.

## Syntax
```
AGGREGATE_FUNCTION (column_1) OVER
 (PARTITION BY column_2 ORDER BY column_3)
  AS new_column_name;
```
#### Create a running total of standard_amt_usd (in the orders table) over order time with no date truncation.
```
SELECT standard_amt_usd,
       SUM(standard_amt_usd) OVER (ORDER BY occurred_at) AS running_total
FROM orders
```

- **Row_number():** Ranking is distinct amongst records even with ties in what the table is ranked against.
- **Rank():** Ranking is the same amongst tied values and ranks skip for subsequent values.
- **Dense_rank():** Ranking is the same amongst tied values and ranks do not skip for subsequent values.

## Aliases Use Case
If you are planning to write multiple window functions that leverage the same PARTITION BY, OVER, and ORDER BY in a single query.
```
SELECT order_id,
       order_total,
       order_price,
       SUM(order_total) OVER monthly_window AS running_monthly_sales,
       COUNT(order_id) OVER monthly_window AS running_monthly orders,
       AVG(order_price) OVER monthly_window AS average_monthly_price
FROM   amazon_sales_db
WHERE  order_date < '2017-01-01'
WINDOW monthly_window AS
       (PARTITION BY month(order_date) ORDER BY order_date);
 ```
 
 ## Comparing a Row to Previous Row - LAG
 - **LAG function :** It returns the value from a previous row to the current row in the table.
 - **LEAD function :**  Return the value from the row following the current row in the table.

```
SELECT account_id,
       standard_sum,
       LAG(standard_sum) OVER(ORDER BY standard_sum) AS lag,
       LEAD(standard_sum) OVER (ORDER BY standard_sum) AS lead,
       standard_sum - LAG(standard_sum) OVER (ORDER BY standard_sum) AS lag_difference,
       LEAD(standard_sum) OVER (ORDER BY standard_sum) - standard_sum AS lead_difference
FROM (
       SELECT account_id,
              SUM(standard_qty) AS standard_sum
       FROM orders
       GROUP BY 1
) sub
```
| account_id	|standard_sum	|lag	|lead	|lag_difference	|lead_difference|
|------------|-------------|----|----|---------------|---------------|
|1901	|0	|	|79|		|79|
|3371|	79|	0	|102	|79	|23|
|1961	|102	|79|	116|	23|	14|
|3401	|116	|102	|117	|14|	1|
|3741	|117	|116	|123|	1|	6|

## Percentiles
Percentiles Syntax
- NTILE + the number of buckets you’d like to create within a column (e.g., 100 buckets would create traditional percentiles, 4 buckets would create quartiles, etc.)
- OVER
- ORDER BY (optional, typically a date column)
- AS + the new column name

```
SELECT
       account_id,
       occurred_at,
       gloss_qty,
       NTILE(2) OVER (PARTITION BY account_id ORDER BY gloss_qty) AS gloss_half
  FROM orders 
 ORDER BY account_id DESC
 ```
