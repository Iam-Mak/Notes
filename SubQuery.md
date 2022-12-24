## subquery
### When do you need to use a subquery?
You need to use a subquery when you have the need to manipulate an existing table to “pseudo-create” a table that is then used as a part of a larger query. In the examples below, existing tables cannot be joined together to solve the problem at hand. Instead, an existing table needs to be manipulated, massaged, or aggregated in some way to then join to another table in the dataset to answer the posed question.

#### Set of Problems:
Identify the top-selling Amazon products in months where sales have exceeded $1m
Existing Table: Amazon daily sales
Subquery Aggregation: Daily to Monthly

|Components	|Subquery	|JOINS|
|-----------|---------|------|
|Combine data from multiple tables into a single result|	Y |	Y|
|Create a flexible view of tables stitched together using a “key”	|	|Y |
|Build an output to use in a later part of the query	|Y |	|
|Subquery Plan: What happens under the hood	|Y	|Y|

<img width="773" alt="image" src="https://user-images.githubusercontent.com/92245436/209431688-c6907eb8-6531-4c66-bcbb-3e8915272556.png">

Placement:
There are four places where subqueries can be inserted within a larger query:

With
Nested
Inline
Scalar
Dependencies:
A subquery can be dependent on the outer query or independent of the outer query.

### Subquery Placement:
- With: This subquery is used when you’d like to “pseudo-create” a table from an existing table and visually scope the temporary table at the top of the larger query.
```
WITH subquery_name (column_name1, ...) AS
 (SELECT ...)
SELECT ...
```
- Nested: This subquery is used when you’d like the temporary table to act as a filter within the larger query, which implies that it often sits within the where clause.
```
SELECT s.s_id, s.s_name, g.final_grade
FROM student s, grades g
WHERE s.s_id = g.s_id
IN (SELECT final_grade
    FROM grades g
    WHERE final_grade >3.7
   );
```
- Inline: This subquery is used in the same fashion as the WITH use case above. However, instead of the temporary table sitting on top of the larger query, it’s embedded within the from clause.
```
SELECT student_name
FROM
  (SELECT student_id, student_name, grade
   FROM student
   WHERE teacher =10)
WHERE grade >80;
```
- Scalar: This subquery is used when you’d like to generate a scalar value to be used as a benchmark of some sort.
```
SELECT s.student_name
  (SELECT AVG(final_score)
   FROM grades g
   WHERE g.student_id = s.student_id) AS
     avg_score
FROM student s;
```
#### Advantages:
**Readability:** With and Nested subqueries are most advantageous for readability.
**Performance:** Scalar subqueries are advantageous for performance and are often used on smaller datasets.
