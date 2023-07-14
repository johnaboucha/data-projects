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

## 06. The LIKE Operator

Select all the films that include a Behind the Scenes special feature.

```
SELECT 
  title,
  special_features
FROM
  film
WHERE
  special_features LIKE '%Behind the Scenes%'
```

## 07. GROUP BY

Get the number of movie titles by rental duration.

```
SELECT
  rental_duration,
  COUNT(title)
FROM
  film
GROUP BY
  rental_duration
```

## 08. Aggregate Functions

Get the count of films, with the average, min, and max rental rate, grouped by replacement cost.

```
SELECT
  replacement_cost,
  COUNT(title) as number_of_films,
  AVG(rental_rate) as average_rental,
  MAX(rental_rate) as most_expensive_rental,
  MIN(rental_rate) as cheapest_rental
FROM
  film
GROUP BY
  replacement_cost
```

## 09. The HAVING Clause

Return a list of customer_ids with less than 15 rentals.

```
SELECT
  customer_id,
  COUNT(rental_id) as total_rentals
FROM
  rental
GROUP BY
  customer_id
HAVING COUNT(rental_id) < 15
```

## 10. The ORDER BY Clause

Return a list of all film titles, with their lengths and rental rates, sorted from longest to shortest.

```
SELECT
  title,
  length,
  rental_rate
FROM
  film
ORDER BY length DESC
```

## 11. The CASE statement

Return first and last names of all customers, and label them as either “store 1 active”, “store 1 inactive”, “store 2 active”, or “store 2 inactive”.

```
SELECT
  first_name,
  last_name,
  CASE
    WHEN active = 1 AND store_id = 1 THEN 'store 1 active'
    WHEN active = 0 AND store_id = 1 THEN 'store 1 inactive'
    WHEN active = 1 AND store_id = 2 THEN 'store 2 active'
    WHEN active = 0 AND store_id = 2 THEN 'store 2 inactive'
    ELSE 'something failed'
  END as is_active
FROM
  customer
```

## 12. COUNT and CASE

Count the number of customers broken down by store_id (in rows) and active status (in columns).

```
SELECT
  store_id,
  COUNT(CASE WHEN active = 1 THEN store_id ELSE NULL END) AS active,
  COUNT(CASE WHEN active = 0 THEN store_id ELSE NULL END) AS inactive
FROM
  customer
GROUP BY
  store_id
```

## 13. Staff Members

Get all staff members, their first and last names, email address, and store they work at.

```
SELECT
  first_name,
  last_name,
  email,
  store_id
FROM
  staff
```

## 14. Inventory

Get inventory counts from each store.

```
SELECT
  store_id,
  COUNT(film_id) as film_count
FROM
  inventory
GROUP BY store_id
```

## 15. Customer count

Count the number of active customers at each store.

```
SELECT
  store_id,
  COUNT(customer_id) as customer_count
FROM
  customer
WHERE
  active = 1
GROUP BY store_id
```

## 16. Customer email count

Count the number of customer emails

```
SELECT
  COUNT(email) as email_count
FROM
  customer
```

## 17. Unique film and category counts

Count the number of unique films at each store. Then count the number of unique categories of films.

```
SELECT
  store_id,
  COUNT(DISTINCT film_id) as unique_films
FROM
  inventory
GROUP BY
  store_id
```

```
SELECT
  COUNT(DISTINCT category_id) as unique_categories
FROM
  film_category
```

## 18. Replacement costs

Get the replacement cost for the least expensive film, the most expensive, and the average of all films.

```
SELECT
  MIN(replacement_cost) as minimum_cost,
  MAX(replacement_cost) as maximum_cost,
  AVG(replacement_cost) as average_cost
FROM
  film
```

## 19. Processing costs

Get the average amount of payment processed and the average.

```
SELECT
  MAX(amount) as maximum_payment,
  AVG(amount) as average_payment
FROM
  payment
```

## 20. Customer info

Get a list of all customer ids, a count of their rentals, and sorted with the highest volume customers at the top.

```
SELECT
  customer_id,
  COUNT(rental_id) as number_of_rentals
FROM
  rental
GROUP BY customer_id
ORDER BY number_of_rentals DESC
```

## 21. INNER JOIN

Return every film title, description, and store_id value, and inventory ID.

```
SELECT
  film.title,
  film.description,
  inventory.store_id,
  inventory.inventory_id
FROM
  film
INNER JOIN inventory
  ON inventory.film_id = film.film_id
```