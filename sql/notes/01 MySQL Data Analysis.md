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

**Comments** in SQL can be added using two dashes “-—“ for single-line comments or “/* */” for multi-line comments.

**Aliases** can be assigned to queries to change the name of a field. Always good to make your code is human readable.

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

You can GROUP By multiple columns at once by listing all the columns and separating with a comma.

```
SELECT
  rating,
  rental_duration
FROM
  film
GROUP BY
  rating,
  rental_duration
```

Aggregate functions can be used with GROUP BY to provide group-level summaries. Examples include:

- COUNT
- COUNT DISTINCT
- MIN
- MAX
- AVG
- SUM

```
SELECT
  rating,
  MIN(length) as shortest_film
FROM
  film
GROUP BY
  rating
```

The **HAVING** clause is used to specify group-level filtering. Instead of acting on individual records, it applies filtering to groups. For instance, if you want to show groups that have counts greater than ten: ```HAVING COUNT(*)>10```.

Another example, to find customers that have had more than 30 rentals:

```
SELECT
  customer_id,
  COUNT(rental_id) as total_rentals
FROM
  rental
GROUP BY
  customer_id
HAVING COUNT(rental_id) > 30
```

The ORDER BY clause specifies the order in which you want your results to display. Ordering can be based on a single or multiple columns. Can also sort by ascending (default) or descending with DESC.

```
SELECT
  customer_id,
  rental_id,
  amount,
  payment_date
FROM
  payment
ORDER BY amount DESC
```

## CASE and COUNT

A **CASE** statement allows you to evaluate a series of if-else statements. It requires at least one WHEN-THEN pair. ELSE is optional. END is required. The gist of it:

```
CASE
  WHEN category IN ('comedy', 'sport') THEN 'Let’s see it!'
  WHEN length > 200 THEN 'too long'
  ELSE 'Let’s just play checkers'
END
```

Another way to think of CASE statements is that they can separate many different values into fewer buckets. Below is a good example of this:

```
SELECT DISTINCT
  length,
  CASE
    WHEN length < 60 THEN 'short movie'
    WHEN length BETWEEN 60 and 90 THEN 'average movie'
    WHEN length > 90 THEN 'eh, too long'
  END as length_bucket
FROM film
```

Logical operators also work inside CASE statements, such as ```WHEN description LIKE '%zombie%' THEN 'gross, no zombies!'

Combining COUNT and CASE with a GROUP BY can yield a pivot table similar to what Excel can do. If we had multiple copies of each film at two different stores, we could show this with the following:

```
SELECT
  film_id,
  COUNT(CASE WHEN store_id = 1 THEN inventory_id ELSE NULL END) AS count_of_store_1_inventory,
  COUNT(CASE WHEN store_ID = 2 THEN inventory_id ELSE NULL END) AS count_of_store_2_inventory
FROM
  inventory
GROUP BY
  film_id
ORDER BY
  film_id
```

## Multiple Tables and Joins

**Normalization** is the process of structuring the tables and columns in a way to minimize redundancy while preserving data integrity. Basically, this involves breaking a single table into multiple, related tables. Primary and foreign keys are used to represent the relationship between the tables.

**Cardinality** means the uniqueness of values in a column. It’s typically used to describe how two tables relate to one another (one-to-one, one-to-many, or many-to-many).

After the data has been normalized into different tables, the **JOIN** statement can be used to query data from multiple, related tables. Common JOIN types include:

- INNER JOIN, returns records that exist in both tables
- LEFT JOIN, returns all records from left table and any matching from the right table
- RIGHT JOIN, returns all records from the right table and any matching left table (not really used)
- FULL OUTER JOIN, returns all records from both tables
- UNION, appends data from one table to the bottom of the other table

An example of the INNER JOIN:

```
SELECT DISTINCT
  inventory.inventory_id
FROM
  inventory
INNER JOIN rental
  ON inventory.inventory_id = rental.inventory_id
LIMIT 5000
```

You can connect to unrelated tables with an intermediary table. This process is called “**bridging**” unrelated tables. The intermediary table will have an ID value that’s shared between the two unconnected tables. Two JOIN statements are needed to do the join. For example:

```
SELECT
  film.film_id,
  film.title,
  category.name as category
FROM
  film
INNER JOIN film_category
  ON film.film_id = film_category.film_id
INNER JOIN category
  ON film_category.category_id = category.category_id
```

It’s possible to use JOIN statements with conditional filters such as WHERE and AND being included after or within the JOIN statement.

The **UNION** statement will only return distinct values. To return duplicates, use UNION ALL. An example of UNION is:

```
SELECT
  'advisor' as type,
  first_name,
  last_name
FROM
  advisor
UNION
SELECT
  'investor' as type,
  first_name,
  last_name
FROM
  investor
```