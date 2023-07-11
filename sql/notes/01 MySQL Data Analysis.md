# MySQL Data Analysis

## Basic SQL Statements

Six of the most basic and commons SQL statements include:

- SELECT, identify columns you wish to select
- FROM, specify the table you want to pull data from
- WHERE, adds record-filtering criteria to the query to filter results
- GROUP BY, allows you to segment the data and do aggregate-level analysis
- HAVING, used in conjunction with GROUP BY and will filter specific groups
- ORDER BY, sorts the query results by column

Always need to include a SELECT and FROM statement in every query.

Select everything:

```
SELECT * FROM rental
```

SQL doesn’t care about line breaks. You can also write the above query as:

```
SELECT *
FROM rental
```

The **USE** statement identifies what schema/database you want to be selecting your data from.

```
USE moviedatabase
```

Select multiple columns from a table by separating column names with columns.

```
SELECT
  customer_id,
  rental_date
FROM
  rental;
```