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