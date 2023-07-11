# Homework

## 01. SELECT & FROM

Select the first name, last name, and email for each of the customers in the movie database.

```
SELECT
  first_name,
  last_name,
  email
FROM
  customer;
```

## 02. SELECT DISTINCT

Use distinct to query the database and find the rental durations for films.

```
SELECT DISTINCT
  rental_duration
FROM
  film;
```

## 03. The WHERE Clause

Select all payments from our first 100 customers (customer ID).

```
SELECT
  customer_id,
  rental_id,
  amount,
  payment_date
FROM
  payment
WHERE
  customer_id < 101;
```

## 04. WHERE and AND

Select all payments from our first 100 customers with payments of at least $5, since January 1, 2006.

```
SELECT
  customer_id,
  rental_id,
  amount,
  payment_date
FROM
  payment
WHERE
  amount >= 5
  AND payment_date > '2006-01-01'
```

## 05. WHERE and OR

Select all payments from the database from customers 42, 53, 60, and 75, where the payments were over $5.

```
SELECT
  *
FROM
  payment
WHERE
  amount > 5
  AND
  (customer_id = 42 OR
  customer_id = 53 OR
  customer_id = 60 OR
  customer_id = 75)
```