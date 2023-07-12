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

The **SELECT DISTINCT** statement will return the unique values in the columns specified. If multiple columns are queried, this will return the distinct _combinations_ of values across those columns.

```
SELECT DISTINCT
  rating
FROM
  film
```

The **WHERE** clause is optional but it’s what you use to filter your query results with conditional logic and the following operators:

- =, equals
- <>, does not equal
- >, greater than
- <, less than
- >=, greater than or equal to
- <=, less than or equal to
- BETWEEN, a range between to values
- LIKE, matching a pattern like a value
- IN(), equals one of these values

```
SELECT
  customer_id,
  rental_id,
  amount,
  payment_date
FROM
  payment
WHERE
  amount = 0.99;
```

You can also add multiple logical conditions to WHERE statements with AND and OR.

```
SELECT
  customer_id,
  amount,
  payment_date
FROM
  payment
WHERE
  amount = 0.99
  AND payment_date > ‘2006-01-01’
```

Next, using **WHERE** and **IN** can be used to reference different values in the same column. It’s a little more elegant than multiple OR clauses.

```
SELECT
  customer_id,
  rental_id,
  amount,
  payment_date
FROM
  payment
WHERE
  amount > 5 AND
  customer_id IN (42,53,60,75)
```

The **LIKE** operator can make your logic more flexible. Instead of querying exact match, you can look for a pattern. For example, we can select all film titles that contain the string “stone”:

```
SELECT 
  *
FROM
  film
WHERE
  title LIKE '%stone%'
```

The underscore character (_) can be used to look for just one character. ```WHERE title LIKE '_art%'``` returns movie titles like Earth Vision and Party Knock.

Take special note how placement of % and _ can alter query results. % will mean any number of characters and the _ will mean only one.

The **GROUP BY** clause specifies how to group the query results. Useful with aggregate functions, like counting how many records there are by some category, like film rating.

```
SELECT 
  rating,
  COUNT(film_id)
FROM
  film
GROUP BY
  rating
```

**Comments** in SQL can be added using two dashes “-—“ for single-line comments or “/* */” for multi-line comments.

**Aliases** can be assigned to queries to change the name of a field. Always good to make your code is human readable.