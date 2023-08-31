# Creating Basic SQL Reports part 2

## Multi-level Aggregation

Create a simple query (no CTE needed) that will show two columns: order_id and item_count. The item_count column should show the sum of quantities of all items in a given order.

```sql
SELECT
  order_id,
  SUM(quantity) as item_count
FROM order_items
GROUP BY order_id
```

We've turned the query you wrote in the previous exercise into a CTE named order_items_counts. Now you need to complete the outer query, which should show a single column named avg_item_count that contains the average number of items in a single order.

```sql
WITH order_items_counts AS (
  SELECT
    order_id,
    SUM(quantity) AS item_count
  FROM order_items
  GROUP BY order_id
)
SELECT
  AVG(item_count) as avg_item_count
FROM order_items_counts
```

What's the average number of products in each category? Show a single value in a column named avg_product_count.

In the inner query, calculate the number of products for each category ID. In the outer query, find the average product count.

```sql
WITH category_count AS (SELECT
  category_id,
  COUNT(product_id) as cat_count
FROM products
GROUP BY category_id)
SELECT
  AVG(cat_count) as avg_product_count
FROM category_count
```

For each employee from the Washington (WA) region, show the average value for all orders they placed. Show the following columns: employee_id, first_name, last_name, and avg_total_price (calculated as the average total order price, before discount).

In the inner query, calculate the value of each order and select it alongside the ID of the employee who processed it. In the outer query, join the CTE with the employees table to show all the required information and filter the employees by region.

```sql
WITH emp_orders AS (
SELECT
  employee_id,
  orders.order_id,
  SUM(unit_price * quantity) as sum_price
FROM orders
JOIN order_items
  ON orders.order_id = order_items.order_id
GROUP BY orders.order_id, employee_id)
SELECT
  employees.employee_id,
  first_name,
  last_name,
  AVG(sum_price) as avg_total_price
FROM employees
LEFT JOIN emp_orders
  ON employees.employee_id = emp_orders.employee_id
WHERE employees.region = 'WA'
GROUP BY employees.employee_id, first_name, last_name
```

For each shipping country, we want to find the average count of unique products in each order. Show the ship_country and avg_distinct_item_count columns. Sort the results by count, in descending order.

In the inner query, find the number of distinct products in each order and select it alongside the ship_country column. In the outer query, apply the proper aggregation.

```sql
WITH items_per_order AS (SELECT
  ship_country,
  order_items.order_id,
  COUNT(DISTINCT product_id) as unique_items
FROM order_items
JOIN orders
  ON order_items.order_id = orders.order_id
GROUP BY ship_country, order_items.order_id)
SELECT
  ship_country,
  AVG(unique_items) as avg_distinct_item_count
FROM items_per_order
GROUP BY ship_country
ORDER BY AVG(unique_items) DESC
```

Count the number of high value and low value customers. If the total price paid by a given customer for all their orders is more than $20,000 before discounts, treat the customer as 'high-value'. Otherwise, treat them as 'low-value'.

Create a report with two columns: category (either 'high-value' or 'low-value') and customer_count.

```sql
WITH customer_sums AS (
  SELECT
    customer_id,
    SUM(unit_price * quantity) as sum_order
  FROM orders
  JOIN order_items
  ON orders.order_id = order_items.order_id
  GROUP BY customer_id
  )
SELECT
  CASE
    WHEN sum_order > 20000 THEN 'high-value'
    ELSE 'low-value'
    END as category,
  COUNT(CASE
    WHEN sum_order > 20000 THEN 1 ELSE 0
  END) as customer_count
FROM customer_sums
GROUP BY CASE
    WHEN sum_order > 20000 THEN 'high-value'
    ELSE 'low-value'
    END
```

What is the average number of products in non-vegetarian (category_id 6 or 8) and vegetarian categories (all other category_id values)? Show two columns: product_type (either 'vegetarian' or 'non-vegetarian') and avg_product_count.

```sql
WITH product_counts AS (SELECT
  category_id,
  COUNT(product_id) as cat_counts
FROM products
GROUP BY category_id)
SELECT
  CASE
    WHEN category_id IN (6,8) THEN 'non-vegetarian'
    ELSE 'vegetarian'
  END as product_type,
  AVG(cat_counts) as avg_product_count
FROM product_counts
GROUP BY
  CASE
    WHEN category_id IN (6,8) THEN 'non-vegetarian'
    ELSE 'vegetarian'
  END
```

For each employee, calculate the average order value (after discount) and then show the minimum average (name the column minimal_average) and the maximum average (name the column maximal_average) values.

```sql
WITH order_totals AS (
  SELECT
    employee_id,
    SUM(unit_price * quantity * (1 - discount)) as order_sum
  FROM orders
  JOIN order_items
    ON order_items.order_id = orders.order_id
  GROUP BY orders.order_id, employee_id
  ),
  
  emp_avgs AS (
  SELECT
    employee_id,
    AVG(order_sum) as order_avg
  FROM order_totals
  GROUP BY employee_id
  )
  
SELECT
  MIN(order_avg) as minimal_average,
  MAX(order_avg) as maximal_average
FROM emp_avgs
```

Among orders shipped to Italy, show all orders that had an above-average total value (before discount). Show the order_id, order_value, and avg_order_value column. The avg_order_value column should show the same average order value for all rows.

```sql
WITH italy_orders as (
  SELECT
    orders.order_id,
    SUM(unit_price * quantity) as order_value
  FROM orders  
  LEFT JOIN order_items
    ON orders.order_id = order_items.order_id
  WHERE ship_country = 'Italy'
  GROUP BY orders.order_id),
  
  avg_order as (
    SELECT
      AVG(order_value) as avg_order_value
    FROM italy_orders
    )
    
SELECT
  order_id,
  order_value,
  avg_order_value
FROM italy_orders, avg_order
WHERE order_value > avg_order_value
```

What's the average number of orders that were processed by a single employee and shipped to the USA or Canada? Show the answer in a column named avg_order_count.

```sql
WITH orders as (SELECT
  employee_id,
  COUNT(order_id) as order_count
FROM orders
WHERE ship_country IN ('USA', 'Canada')
GROUP BY employee_id)

SELECT
  AVG(order_count) as avg_order_count
FROM orders
```

Find the average order value (after discount) for each customer. Show the customer_id and avg_discounted_price columns.

```sql
WITH customer_orders as (
SELECT
  orders.order_id,
  customer_id,
  SUM(unit_price * quantity * (1-discount)) as order_value
FROM order_items
JOIN orders
  ON orders.order_id = order_items.order_id
GROUP BY orders.order_id, customer_id
)
  
SELECT
  customer_id,
  AVG(order_value) as avg_discounted_price
FROM customer_orders
GROUP BY customer_id
```

We want to see if cheaper products are currently being ordered in larger quantities.

Create a report with two columns: price_category (which will contain either 'cheap' for products with a maximum unit_price of 20.0 or 'expensive' otherwise) and avg_products_on_order (the average number of units on order for a given price category).

```sql
SELECT
  CASE
    WHEN unit_price <= 20.0 THEN 'cheap'
    ELSE 'expensive'
  END as price_category,
  AVG(units_on_order) as avg_products_on_order
FROM products
GROUP BY CASE
  WHEN unit_price <= 20.0 THEN 'cheap'
  ELSE 'expensive'
END
```

## Multiple Metrics in One Report

We want to see each customer's ID alongside the number of orders they placed and the total revenue (after discount) that their purchases generated generated for us. Show three columns:

- The customer's ID (customer_id).
- The number of orders (as order_count).
- The total price paid for all orders after discounts (as total_revenue_after_discount).

```sql
SELECT
  customer_id,
  COUNT(DISTINCT orders.order_id) as order_count,
  SUM(unit_price * quantity * (1 - discount)) as total_revenue_after_discount
FROM orders
JOIN order_items
ON orders.order_id = order_items.order_id
GROUP BY customer_id
```

For each employee, determine their performance for 2016: Compute the total number and the total revenue for orders processed by each employee. Show the employee's first name (first_name), last name (last_name), and the two metrics in columns named order_count (total number of orders processed by the employee) and order_revenue (total revenue for orders processed by the employee).

```sql
SELECT
  first_name,
  last_name,
  COUNT(DISTINCT orders.order_id) as order_count,
  SUM(unit_price * quantity * (1 - discount)) as order_revenue
FROM employees
LEFT JOIN orders
  ON orders.employee_id = employees.employee_id
LEFT JOIN order_items
  ON orders.order_id = order_items.order_id
WHERE order_date BETWEEN '2016-01-01' AND '2016-12-31'
GROUP BY employees.employee_id, first_name, last_name
```

For each category, show the number of products in stock (i.e., products where units_in_stock > 0) and the number of products not in stock. The report should contain three columns:

- category_name
- products_in_stock
- products_not_in_stock

```sql
SELECT
  category_name,
  COUNT(CASE
    WHEN units_in_stock > 0 THEN 1 END) as products_in_stock,
  COUNT(CASE
    WHEN units_in_stock = 0 THEN 1 END) as products_not_in_stock
FROM products
JOIN categories
  ON categories.category_id = products.category_id
GROUP BY category_name
```

For each order, determine how many line items (unique products) were discounted and how many weren't. Show three columns:

- The order ID (order_id).
- The number of line items that were not discounted (as full_price_product_count).
- The number of line items that were discounted (as discount_product_count)

```sql
SELECT
  order_id,
  COUNT(CASE
    WHEN discount = 0.00 THEN 1 END) as full_price_product_count,
  COUNT(CASE
    WHEN discount > 0.00 THEN 1 END) as discount_product_count
FROM order_items
GROUP BY order_id
```

We want to find the ratio of the revenue from all discounted items to the total revenue from all items. We'll do this in steps too.

First, show two columns:

1. discounted_revenue – the revenue (after discount) from all discounted line items in all orders.
1. total_revenue – the total revenue (after discount) from line items in all orders.

```sql
SELECT
  SUM(CASE
    WHEN discount != 0.00 THEN unit_price * quantity * (1 - discount)
    END) as discounted_revenue,
  SUM(unit_price * quantity * (1 - discount)) as total_revenue
FROM order_items
```

Add a third column to the previous example: discounted_ratio. It should contain the ratio of discounted line items (from column 1) to all line items (from column 2).

```sql
WITH order_sums as (SELECT
  SUM(CASE
    WHEN discount > 0
      THEN unit_price * quantity * (1 - discount)
  END) AS discounted_revenue,
  SUM(unit_price * quantity * (1 - discount)) AS total_revenue
FROM order_items)

SELECT
  discounted_revenue,
  total_revenue,
  discounted_revenue / CAST(total_revenue as decimal) as discounted_ratio
FROM order_sums
```

What is the percentage of discontinued items at Northwind? Show three columns: count_discontinued, count_all, and percentage_discontinued. Round the last column to two decimal places.

```sql
WITH order_counts as (SELECT
  COUNT(CASE
    WHEN discontinued = 't' THEN 1 END) as count_discontinued,
  COUNT(product_id) as count_all
FROM products)

SELECT
  count_discontinued,
  count_all,
  ROUND(count_discontinued / CAST(count_all as decimal)*100, 2) as percentage_discontinued
FROM order_counts
```

For each employee, find the percentage of all orders they processed that were placed by customers in France. Show five columns: first_name, last_name, count_france, count_all, and percentage_france (rounded to one decimal point).

```sql
SELECT
  first_name,
  last_name,
  COUNT(CASE
       WHEN ship_country = 'France' THEN order_id END) as count_france,
  COUNT(order_id) as count_all,
  ROUND(COUNT(CASE
       WHEN customers.country = 'France' THEN order_id END) / 
       CAST(COUNT(order_id) as decimal) * 100, 1) as percentage_france
FROM employees
LEFT JOIN orders
  ON employees.employee_id = orders.employee_id
LEFT JOIN customers
  ON customers.customer_id = orders.customer_id
GROUP BY employees.employee_id, first_name, last_name
```

We want to see each employee alongside the number of orders they processed in 2017 and the percentage of all orders from 2017 that they generated. Show the following columns:

1. employee_id.
1. first_name.
1. last_name.
1. order_count – the number of orders processed by that employee in 2017.
1. order_count_percentage – the percentage of orders from 2017 processed by that employee.

Round the value of the last column to two decimal places.

```sql
WITH total_orders as (SELECT
  COUNT(order_id) as total_sales
FROM orders
WHERE order_date BETWEEN '2017-01-01' AND '2017-12-31')

SELECT
  employees.employee_id,
  first_name,
  last_name,
  COUNT(order_id) as order_count,
  ROUND(COUNT(order_id) / CAST(total_sales as decimal)*100,2) as order_count_percentage
FROM total_orders, employees
JOIN orders
  ON employees.employee_id = orders.employee_id
WHERE order_date BETWEEN '2017-01-01' AND '2017-12-31'
GROUP BY employees.employee_id, first_name, last_name, total_orders.total_sales
```

For each country, find the percentage of revenue generated by orders shipped to it in 2018. Show three columns:

1. ship_country.
1. revenue – the total revenue generated by all orders shipped to that country in 2018.
1. revenue_percentage – the percentage of that year's revenue generated by orders shipped to that country in 2018.

Round the percentage to two decimal places. Consider revenue without discount. Order the results in descending order by the revenue column.

```sql
WITH totals as (SELECT
  SUM(unit_price * quantity) as total_revenue
FROM order_items
JOIN orders
  ON order_items.order_id = orders.order_id
WHERE shipped_date BETWEEN '2018-01-01' AND '2018-12-31'
)
SELECT
  ship_country,
  SUM(unit_price * quantity) as revenue,
  ROUND(SUM(unit_price * quantity)
  / CAST(total_revenue as decimal) * 100,2) as revenue_percentage
FROM totals, orders
JOIN order_items
  ON orders.order_id = order_items.order_id
WHERE shipped_date BETWEEN '2018-01-01' AND '2018-12-31'
GROUP BY ship_country, total_revenue
ORDER BY revenue DESC
```