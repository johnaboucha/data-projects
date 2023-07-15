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

## 22. LEFT JOIN

Return a list of all films and the number of actors in each.

```
SELECT
  film.title,
  COUNT(film_actor.actor_id) as number_of_actors
FROM
  film
LEFT JOIN film_actor
  ON film.film_id = film_actor.film_id
GROUP BY film.title
ORDER BY number_of_actors DESC
```

## 23. Bridging Tables

Return a list of all actors and the films they appeared in.

```
SELECT
  actor.first_name,
  actor.last_name,
  film.title
FROM
  actor
INNER JOIN film_actor
  ON film_actor.actor_id = actor.actor_id
INNER JOIN film
  ON film_actor.film_id = film.film_id
```

## 24. Multi-Condition Joins

Return a list of distinct titles, their description, and currently available at store 2.

```
SELECT
  distinct title,
  description
FROM
  film
INNER JOIN inventory
  ON film.film_id = inventory.film_id
  AND inventory.store_id = 2
```

## 25. Union operator

List all staff and advisor names, and include a column noting whether they are a staff member or advisor.

```
SELECT
  first_name,
  last_name,
  'staff' as type
FROM
  staff
UNION
SELECT
  first_name,
  last_name,
  'advisor' as type
FROM
  advisor
```

## 26. Staff members

Return a list of staff members and the addresses of their store.

```
SELECT
  first_name,
  last_name,
  staff.store_id,
  address.address,
  address.district,
  city.city,
  country.country
FROM
  staff
LEFT JOIN store
  ON store.store_id = staff.store_id
LEFT JOIN address
  ON store.address_id = address.address_id
LEFT JOIN city
  ON city.city_id = address.city_id
LEFT JOIN country
  ON country.country_id = city.country_id
```

## 27. Inventory

Return a list of all the inventory, with their store ID, inventory ID, title of film, film’s rating, rental rate, and replacement cost.

```
SELECT
  film.film_id,
  store.store_id,
  title,
  rating,
  rental_rate,
  replacement_cost
FROM
  film
INNER JOIN inventory
  ON inventory.film_id = film.film_id
LEFT JOIN store
  ON store.store_id = inventory.store_id
```

## 28. Inventory by store

Return a list of inventory by store by rating.

```
SELECT
  COUNT(film.film_id) as count,
  inventory.store_id as store,
  rating
FROM
  film
INNER JOIN inventory
  ON inventory.film_id = film.film_id
GROUP BY rating, store
ORDER BY store, rating
```

## 29. Film diversity

Return the number of films, along with the average replacement cost, and total replacement cost by store and per category.

```
SELECT
  COUNT(film.film_id) as number_of_films,
  AVG(replacement_cost) as average_cost,
  inventory.store_id as store,
  category.name as category_name
FROM
  film
INNER JOIN inventory
  ON inventory.film_id = film.film_id
LEFT JOIN film_category
  ON film_category.film_id = film.film_id
LEFT JOIN category
  ON category.category_id = film_category.category_id
GROUP BY store, category_name
```

## 30. Customer list

Return a list of all the customers, which store they go to, active status, full address, city, and country.

```
SELECT
  first_name,
  last_name,
  store_id,
  active,
  address.address,
  city.city,
  country.country
FROM
  customer
LEFT JOIN address
  ON customer.address_id = address.address_id
LEFT JOIN city
  ON address.city_id = city.city_id
LEFT JOIN country
  ON city.country_id = country.country_id
```

## 31. Customer spends

Return a list of customers, their lifetime rentals, and sum of payments, sorted with highest sum of payments first.

```
SELECT
  customer.customer_id,
  first_name,
  last_name,
  SUM(payment.amount) as amount
FROM
  customer
INNER JOIN payment
  on payment.customer_id = customer.customer_id
GROUP BY customer.customer_id
ORDER BY amount DESC
```

## 32. Investors and advisors

Return a list of advisors and investors, and if an investor, also include what company they work with.

```
SELECT
  first_name,
  last_name,
  'advisor' as type,
  '' as company
FROM
  advisor
UNION
SELECT
  first_name,
  last_name,
  'investor' as type,
  company_name
FROM
  investor
```

## 33. Most-awarded actors

Of all the actors with 3 awards, what % of them do we carry a film?

```
SELECT
  '3 awards' as number_of_awards,
  COUNT(actor_id) / COUNT(*) as percentage
FROM
  actor_award
WHERE
  LENGTH(actor_award.awards) - LENGTH(REPLACE(actor_award.awards,",","")) + 1 = 3
UNION
SELECT
 '2 awards' as number_of_awards,
  COUNT(actor_id) / COUNT(*) as percentage
FROM
  actor_award
WHERE
  LENGTH(actor_award.awards) - LENGTH(REPLACE(actor_award.awards,",","")) + 1 = 2
UNION
SELECT
 '1 award' as number_of_awards,
  COUNT(actor_id) / COUNT(*) as percentage
FROM
  actor_award
WHERE
  LENGTH(actor_award.awards) - LENGTH(REPLACE(actor_award.awards,",","")) + 1 = 1
```