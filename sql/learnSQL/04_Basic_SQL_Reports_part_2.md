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