# Creating Basic SQL Reports part 1

Database used is based on Microsoft’s Northwind dataset, a fictional store and its customers, suppliers, and orders. Find it on [GitHub](https://github.com/Microsoft/sql-server-samples/tree/master/samples/databases/northwind-pubs).

Select each customer's ID, company name, contact name, contact title, city, and country. Order the results by the name of the country.

```sql
SELECT
  customer_id,
  company_name,
  contact_name,
  contact_title,
  city,
  country
FROM customers
ORDER BY country
```

For each product, display its name (product_name), the name of the category it belongs to (category_name), quantity per unit (quantity_per_unit), the unit price (unit_price), and the number of units in stock (units_in_stock). Order the results by unit price.

```sql
SELECT
  product_name,
  category_name,
  quantity_per_unit,
  unit_price,
  units_in_stock
FROM products
LEFT JOIN categories
  ON products.category_id = categories.category_id
ORDER BY unit_price
```

We'd like to see information about all the suppliers who provide the store four or more different products. Show the following columns: supplier_id, company_name, and products_count (the number of products supplied).

```sql
SELECT
  suppliers.supplier_id,
  company_name,
  COUNT(product_id) as products_count
FROM suppliers
LEFT JOIN products
  ON suppliers.supplier_id = products.supplier_id
GROUP BY suppliers.supplier_id, company_name
HAVING COUNT(product_id) >= 4
```

Display the list of products purchased in the order with ID equal to 10250. Show the following information: product name (product_name), the quantity of the product ordered (quantity), the unit price (unit_price from the order_items table), the discount (discount), and the order_date. Order the items by product name.

```sql
SELECT 
  product_name,
  quantity,
  order_items.unit_price,
  discount,
  order_date
FROM products
JOIN order_items
  ON products.product_id = order_items.product_id
JOIN orders
  on order_items.order_id = orders.order_id
WHERE orders.order_id = 10250
ORDER BY product_name
```

## Summarizing Data in SQL

Show the following information related to all items with order_id = 10248: the product name, the unit price (taken from the order_items table), the quantity, and the name of the supplier's company (as supplier_name).

```sql
SELECT
  product_name,
  order_items.unit_price,
  quantity,
  company_name as supplier_name
FROM orders
JOIN order_items
  ON order_items.order_id = orders.order_id
JOIN products
  ON order_items.product_id = products.product_id
JOIN suppliers
  ON products.supplier_id = suppliers.supplier_id
WHERE orders.order_id = 10248
```

Show the following information for each product: the product name, the company name of the product supplier (use the suppliers table), the category name, the unit price, and the quantity per unit.

```sql
SELECT
  product_name,
  company_name,
  category_name,
  unit_price,
  quantity_per_unit
FROM products
JOIN suppliers
  ON products.supplier_id = suppliers.supplier_id
JOIN categories
  ON products.category_id = categories.category_id
```

Count the number of employees hired in 2013. Name the result number_of_employees.

```sql
SELECT
  COUNT(employee_id) as number_of_employees
FROM employees
WHERE hire_date BETWEEN '2013-01-01' AND '2013-12-31'
```

Show each supplier_id alongside the company_name and the number of products they supply (as the products_count column). Use the products and suppliers tables.

```sql
SELECT
  suppliers.supplier_id,
  company_name,
  COUNT(product_id) as products_count
FROM suppliers
JOIN products
  ON suppliers.supplier_id = products.supplier_id
GROUP BY suppliers.supplier_id, company_name
```

We want to know the number of orders processed by each employee. Show the following columns: employee_id, first_name, last_name, and the number of orders processed as orders_count.

```sql
SELECT
  employees.employee_id,
  first_name,
  last_name,
  COUNT(order_id) as orders_count
FROM employees
JOIN orders
  on employees.employee_id = orders.employee_id
GROUP BY employees.employee_id, first_name, last_name
```

How much are the products in stock in each category worth? Show three columns: category_id, category_name, and category_total_value. You'll calculate the third column as the sum of unit prices multiplied by the number of units in stock for all products in the given category.

```sql
SELECT
  categories.category_id,
  category_name,
  SUM(unit_price * units_in_stock) as category_total_value
FROM categories
JOIN products
  ON categories.category_id = products.category_id
GROUP BY categories.category_id, category_name
```

Count the number of orders placed by each customer. Show the customer_id, company_name, and orders_count columns.

```sql
SELECT
  customers.customer_id,
  company_name,
  COUNT(order_id) as orders_count
FROM customers
JOIN orders
  ON customers.customer_id = orders.customer_id
GROUP BY customers.customer_id, company_name
```

Which customers paid the most for orders made in June 2016 or July 2016? Show two columns:

1. company_name
1. total_paid, calculated as the total price (after discount) paid for all orders made by a given customer in June 2016 or July 2016.

Sort the results by total_paid in descending order.

```sql
SELECT
  company_name,
  SUM(unit_price * quantity * (1 - discount)) as total_paid
FROM customers
JOIN orders
  ON customers.customer_id = orders.customer_id
JOIN order_items
  ON orders.order_id = order_items.order_id
WHERE order_date BETWEEN '2016-06-01' AND '2016-07-31'
GROUP BY company_name
ORDER BY total_paid DESC
```