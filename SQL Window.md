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
