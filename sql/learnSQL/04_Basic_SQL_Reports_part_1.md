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

Count the total number of customers and all those with a fax number. Show two columns: all_customers_count and customers_with_fax_count.

```sql
SELECT
  COUNT(*) as all_customers_count,
  COUNT(fax) as customers_with_fax_count
FROM customers
```

Find the total number of products provided by each supplier. Show the company_name and products_count (the number of products supplied) columns. Include suppliers that haven't provided any products.

```sql
SELECT
  company_name,
  COUNT(product_id) as products_count
FROM suppliers
LEFT JOIN products
  ON suppliers.supplier_id = products.supplier_id
GROUP BY company_name
```

Show the number of unique customers (as number_of_customers) that had orders shipped to Spain.

```sql
SELECT
  COUNT(DISTINCT customer_id) as number_of_customers
FROM orders
WHERE ship_country = 'Spain'
```

Find the total number of products supplied by each supplier. Show the following columns: supplier_id, company_name, and products_supplied_count (the number of products supplied by that company). Show also suppliers that supply no products.

```sql
SELECT
  suppliers.supplier_id,
  company_name,
  COUNT(product_id) as products_supplied_count
FROM suppliers
LEFT JOIN products
  ON suppliers.supplier_id = products.supplier_id
GROUP BY suppliers.supplier_id, company_name
```

How many distinct products are there in all orders shipped to France? Name the result distinct_products.

```sql
SELECT
  COUNT(DISTINCT product_id) as distinct_products
FROM order_items
JOIN orders
  ON order_items.order_id = orders.order_id
WHERE ship_country = 'France'
```

Show three kinds of information about product suppliers:

1. all_suppliers (the total number of suppliers)
1. suppliers_region_assigned (the total number of suppliers who are assigned to a region)
1. unique_supplier_regions (the number of unique regions suppliers are assigned to)

```sql
SELECT
  COUNT(supplier_id) as all_suppliers,
  COUNT(region) as suppliers_region_assigned,
  COUNT(DISTINCT region) as unique_supplier_regions
FROM suppliers
```

## Classifying Data with CASE WHEN and GROUP BY

We want to create a report measuring the level of experience each Northwind employee has with the company. Show the first_name, last_name, hire_date, and experience columns for each employee. The experience column should display the following values:

- 'junior' for employees hired after Jan. 1, 2014.
- 'middle' for employees hired after Jan. 1, 2013 but before Jan. 1, 2014.
- 'senior' for employees hired on or before Jan. 1, 2013.

```sql
SELECT
  first_name,
  last_name,
  hire_date,
  CASE
    WHEN hire_date > '2014-01-01' THEN 'junior'
    WHEN hire_date > '2013-01-01' THEN 'middle'
    WHEN hire_date <= '2013-01-01' THEN 'senior'
  END as experience
FROM employees
```

We want to show the following basic customer information (from the customers table):

1. customer_id
1. company_name
1. country
1. language

The value of the language column will be decided by the following rules:

- 'German' for companies from Germany, Switzerland, and Austria.
- 'English' for companies from the UK, Canada, the USA, and Ireland.
- 'Other' for all other countries.

```sql
SELECT
  customer_id,
  company_name,
  country,
  CASE
    WHEN country IN ('Germany', 'Switzerland', 'Austria') THEN 'German'
    WHEN country IN ('UK', 'Canada', 'USA', 'Ireland') THEN 'English'
    ELSE 'Other'
  END as language
FROM customers
```

Let's create a report that will divide all products into vegetarian and non-vegetarian categories. For each product, show the following columns:

1. product_name
1. category_name
1. diet_type:
- 'Non-vegetarian' for products from the categories 'Meat/Poultry' and 'Seafood'.
- 'Vegetarian' for any other category.

```sql
SELECT
  product_name,
  category_name,
  CASE
    WHEN category_name IN ('Meat/Poultry', 'Seafood') THEN 'Non-vegetarian'
    ELSE 'Vegetarian'
  END as diet_type
FROM products
LEFT JOIN categories
  ON products.category_id = categories.category_id
```

Create a report that shows the number of products supplied from a specific continent. Display two columns: supplier_continent and product_count. The supplier_continent column should have the following values:

- 'North America' for products supplied from 'USA' and 'Canada'.
- 'Asia' for products from 'Japan' and 'Singapore'.
- 'Other' for other countries.

```sql
SELECT
  CASE
    WHEN country IN ('USA', 'Canada') THEN 'North America'
    WHEN country IN ('Japan', 'Singapore') THEN 'Asia'
  ELSE 'Other' END as supplier_continent,
  COUNT(product_id) as product_count
FROM suppliers
JOIN products
  ON suppliers.supplier_id = products.supplier_id
GROUP BY
  CASE
    WHEN country IN ('USA', 'Canada') THEN 'North America'
    WHEN country IN ('Japan', 'Singapore') THEN 'Asia'
  ELSE 'Other' END
```

We want to create a simple report that will show the number of young and old employees at Northwind. Show two columns: age and employee_count.

The age column has the following values:

- 'young' for people born after Jan. 1, 1980.
- 'old' for all other employees.

```sql
SELECT
  CASE
    WHEN birth_date > '1980-01-01' THEN 'young'
    ELSE 'old'
   END as age,
  COUNT(employee_id) as employee_count
FROM employees
GROUP BY
  CASE
    WHEN birth_date > '1980-01-01' THEN 'young'
    ELSE 'old'
  END 
```

How many customers are represented by owners (contact_title = 'Owner'), and how many aren't? Show two columns with appropriate values: represented_by_owner and not_represented_by_owner.

```sql
SELECT
  COUNT(
    CASE
      WHEN contact_title = 'Owner' THEN 1
    END) as represented_by_owner,
  COUNT(
    CASE
      WHEN contact_title != 'Owner' THEN 1
    END) as not_represented_by_owner
FROM customers
```

Washington (WA) is Northwind's primary region. How many orders have been processed by employees in the WA region, and how many by employees in other regions? Show two columns with their respective counts: orders_wa_employees and orders_not_wa_employees.

```sql
SELECT
  COUNT(
    CASE WHEN region = 'WA' THEN 1
    END) as orders_wa_employees,
  COUNT(
    CASE WHEN region != 'WA' THEN 1
    END) as orders_not_wa_employees
FROM employees
JOIN orders
  ON employees.employee_id = orders.employee_id
```

We need a report that will show the number of products with high and low availability in all product categories. Show three columns: category_name, high_availability (count the products with more than 30 units in stock) and low_availability (count the products with 30 or fewer units in stock).

```sql
SELECT
  category_name,
  COUNT(
    CASE WHEN units_in_stock > 30 THEN 1
    END) as high_availability,
  COUNT(
    CASE WHEN units_in_stock <= 30 THEN 1
    END) as low_availability
FROM products
JOIN categories
  ON products.category_id = categories.category_id
GROUP BY category_name
```

There have been a lot of orders shipped to France. Of these, how many order items were sold at full price and how many were discounted? Show two columns with the respective counts: full_price and discounted_price.

```sql
SELECT
  SUM(
    CASE WHEN discount = 0.00 THEN 1
    END) as full_price,
  SUM(
    CASE WHEN discount != 0.00 THEN 1
    END) as discounted_price
FROM order_items
JOIN orders
  ON order_items.order_id = orders.order_id
WHERE ship_country = 'France'
```

This time, we want a report that will show each supplier alongside their number of units in stock and their number of expensive units in stock. Show four columns: supplier_id, company_name, all_units (all units in stock supplied by that supplier), and expensive_units (units in stock with a unit price over 40.0, supplied by that supplier).

```sql
SELECT
  suppliers.supplier_id,
  company_name,
  SUM(units_in_stock) as all_units,
  SUM(
    CASE
      WHEN unit_price > 40.00 THEN units_in_stock ELSE 0
    END) as expensive_units
FROM products
JOIN suppliers
  ON products.supplier_id = suppliers.supplier_id
GROUP BY suppliers.supplier_id, company_name
```

For each product, show the following columns: product_id, product_name, unit_price, and price_level. The price_level column should show one of the following values:

- 'expensive' for products with a unit price above 100.
- 'average' for products with a unit price above 40 but no more than 100.
- 'cheap' for other products.

```sql
SELECT
  product_id,
  product_name,
  unit_price,
  CASE
    WHEN unit_price > 100 THEN 'expensive'
    WHEN unit_price > 40 THEN 'average'
    ELSE 'cheap'
  END as price_level
FROM products
```

We would like to categorize all orders based on their total price (before any discount). For each order, show the following columns:

1. order_id
1. total_price (calculated before discount)
1. price_group, which should have the following values:

- 'high' for a total price over $2,000.
- 'average' for a total price between $600 and $2,000, both inclusive.
- 'low' for a total price under $600.

```sql
SELECT
  order_id,
  SUM(unit_price * quantity) as total_price,
  CASE
    WHEN SUM(unit_price * quantity) > 2000 THEN 'high'
    WHEN SUM(unit_price * quantity) > 600 THEN 'average'
    ELSE 'low'
  END as price_group
FROM order_items
GROUP BY order_id
```

Group all orders based on the freight column. Show three columns in your report:

- low_freight – the number of orders where the freight value is less than 40.0.
- avg_freight – the number of orders where the freight value is greater than equal to or 40.0 but less than 80.0.
- high_freight – the number of orders where the freight value is greater than equal to or 80.0.

```sql
SELECT
  COUNT(
    CASE
      WHEN freight < 40.0 THEN 1
    END) as low_freight,
  COUNT(
    CASE
      WHEN freight >= 40.0 AND freight < 80.0 THEN 1
    END) as avg_freight,
  COUNT(
    CASE
      WHEN freight > 80.0 THEN 1
    END) as high_freight
FROM orders
```