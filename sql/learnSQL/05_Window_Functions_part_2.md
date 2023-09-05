# Window Functions - part 2

## Window Frame

_product_

| column name | example value |
| ---- | ---- |
| id | 1 |
| name | Frozen Yogurt |
| introduced | 2016-01-26 |

_stock_change_

| column name | example value |
| ---- | ---- |
| id | 1 |
| product_id | 5 |
| quantity | -90 |
| changed | 2016-09-11 |

_single_order_

| column name | example value |
| ---- | ---- |
| id | 1 |
| placed | 2016-07-10 |
| total_price | 3876.76 |

_order_position_

| column name | example value |
| ---- | ---- |
| id | 1 |
| product_id | 1 |
| order_id | 9 |
| quantity | 7 |

For each order, show its id, the placed date, and the third column which will count the number of orders up to the current order when sorted by the placed date.

```sql
SELECT
  id,
  placed,
  COUNT(id) OVER(ORDER BY placed ROWS between UNBOUNDED PRECEDING and CURRENT ROW)
FROM single_order
```

Warehouse workers always need to pick the products for orders by hand and one by one. For positions with order_id = 5, calculate the remaining sum of all the products to pick. For each position from that order, show its id, the id of the product, the quantity and the quantity of the remaining items (including the current row) when sorted by the id in the ascending order.

```sql
SELECT
  id,
  product_id,
  quantity,
  SUM(quantity) OVER(ORDER BY order_id ROWS between CURRENT ROW and UNBOUNDED FOLLOWING)
FROM order_position
WHERE order_id = 5
```

For each product, show its id, name, introduced date and the count of products introduced up to that point.

```sql
SELECT
  id,
  name,
  introduced,
  COUNT(id) OVER(ORDER BY introduced ROWS between UNBOUNDED PRECEDING and CURRENT ROW)
FROM product
```

Now, for each single_order, show its placed date, total_price, the average price calculated by taking 2 previous orders, the current order and 2 following orders (in terms of the placed date) and the ratio of the total_price to the average price calculated as before.

```sql
SELECT
  placed,
  total_price,
  AVG(total_price) OVER(ORDER BY placed ROWS between 2 PRECEDING and 2 FOLLOWING),
  total_price / AVG(total_price)
    OVER(ORDER BY placed ROWS between 2 PRECEDING and 2 FOLLOWING)
FROM single_order 
```