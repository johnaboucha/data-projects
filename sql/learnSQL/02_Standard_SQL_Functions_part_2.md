# Standard SQL Functions Part 2

## Nulls

_product_

| column name  | example value |
| ----| ---- |
| id   | 1 |
| category | bedroom |
| name	 | Vaatekaappi |
| type | wardrobe |
|	price	| 299.99 |
| launch_date | 2015-08-01 |
| description | lorem ipsum |
| production_area | UE |
| shipping_cost | 10.00 |

_advertisement_

| id  | product_id | country | start_date | end_date |
| ----| ---- | ---- | ---- | ---- |
| 1  | 1 | Poland | 2016-04-10 | null |

Select name for all products whose launch_date is not a null.

```sql
SELECT name
FROM product
WHERE launch_date IS NOT NULL
```

Now, select name for all the products whose category is NULL.

```sql
SELECT name
FROM product
WHERE category IS NULL
```

Select the name for all the products with a price different than 299.99. Include NULLs!

```sql
SELECT name
FROM product
WHERE price != 299.99
  OR price IS NULL
```

Select the name for all products together with their categories and types. Exclude rows with category 'kitchen' and those rows which have no category set.

```sql
SELECT
  name,
  category,
  type
FROM product
WHERE category != 'kitchen'
```

Mr Amund released a certain series of products on April 30, 2014. Now, he would like to get all the names, categories and types of those products which were introduced any time later or which will be introduced in the future (i.e., they don't have a launch_date yet). Write the proper query.

```sql
SELECT
  name,
  category,
  type
FROM product
WHERE launch_date > '2014-04-30'
  OR launch_date IS NULL
```

Select all the columns for products from categories: kitchen, bathroom, or with unknown category.

```sql
SELECT *
FROM product
WHERE category IN ('kitchen', 'bathroom')
  OR category IS NULL
```

A new tax has been imposed on all the products, which is 2% of the final price. Show all product names with their old price (column name old_price) and new price rounded to two decimal points (column name new_price). Note what happens when there is no price.

```sql
SELECT
  name,
  price as old_price,
  round(price * 1.02,2) as new_price
FROM
  product
```

Show the names of all products shorter than 8 characters or those which have a NULL name. Show the description as the second column so that Mr Amund can think of a (short) name if it's missing for a certain product before the advertisement is published.

```sql
SELECT
  name,
  description
FROM product
WHERE length(name) < 8
  OR name IS NULL
```

Select the names and categories for all products. If any of the columns is a NULL, write 'n/a' instead. Name the columns name and category.

```sql
SELECT
  COALESCE(name, 'n/a') as name,
  COALESCE(category, 'n/a') as category
FROM product
```

Show the following sentence: Product X is made in Y. Where X is the name and Y is the production_area. If the name is not provided, write 'unknown name' instead. If the production_area is not provided, write 'unknown area'. Name the column sentence.

```sql
SELECT
  'Product ' || COALESCE(name, 'unknown name') ||
  ' is made in ' || COALESCE(production_area, 'unknown area') || '.' as sentence
FROM product
```

The company organized a discount campaign on all prices: -10%.

Show the names of all products together with their price and the new prices (name the column new_price). If there is no price, show 0.00 in the column new_price. Round the result to two decimal points.

```sql
SELECT
  name,
  price,
  round(COALESCE(price, 0) * .9,2) as new_price
FROM product
```

Show the name of each product together with its type. When the type is unknown, show the category. When the category is missing too, show 'No clue what that is'. Name the column type.

```sql
SELECT
  name,
  COALESCE(type, category, 'No clue what that is') as type
FROM product
```

Today, the customers at Highland Furniture get a special offer: the price of each product is decreased by 1.99! This means that some products may even be free!

Our customer has $1,000.00 and would like to know how many products of each kind they could buy.

Show the name of each product and a second column named quantity: Divide the sum of 1000.00 by the new price of each product. Watch out for division by 0 and return a NULL instead.

```sql
SELECT
  name,
  1000.00 / NULLIF(price - 1.99,0) as quantity
FROM product
```

Show the name of each product and the column ratio calculated in the following way: the price of each product in relation to its shipping_cost in percent, rounded to an integer value (e.g. 31).

If the shipping_cost is 0.00, show NULL instead.

```sql
SELECT
  name,
  round(price / NULLIF (shipping_cost, 0.00) * 100) as ratio
FROM product
```

The promotion is as follows: every customer who orders a product and picks it up on their own (i.e., no shipping required) can buy it at a special price: the initial price MINUS the shipping_cost! If the shipping_cost is greater than the price itself, then the customer still pays the difference (i.e. the absolute value of price - shipping_cost).

Our customer has $1,000.00 again and wants to know how many products of each kind they could buy. Show each product name with the column quantity which calculates the number of products. Drop the decimal part. If you get a price of 0.00, show NULL instead.

```sql
SELECT
  name,
  floor(1000.00 / NULLIF(abs(price - shipping_cost),0.00)) as quantity
FROM product
```

Select the name and launch_date of all products for which the launch date is not February 25, 2015.

```sql
SELECT
  name,
  launch_date
FROM product
WHERE launch_date != '2015-02-25'
  OR launch_date IS NULL
```

Show the name of all products together with their price. If the price is equal to 19.99 change it to NULL. However, make sure there aren't any NULLs in the price column, show 'unknown' instead of them. Make sure the latter column is named price.

```sql
SELECT
  name,
  COALESCE(CAST(NULLIF(price, 19.99) as varchar), 'unknown', cast(price as varchar)) as price
FROM product
```

Show the id, name and category of all products in the descending order of names.

```sql
SELECT
  id,
  name,
  category
FROM product
ORDER BY name DESC
```

For each advertisement, show its id, country, start_date and end_date together with the name of the product advertised.

If there is no start_date or no end_date, show 'n/a' instead. If there is no name of the product, show 'no name'. Order the results by the start_date, with the oldest dates shown first. The column names should be: id, country, start_date, end_date, and name.

```sql
SELECT
  advertisement.id,
  country,
  COALESCE(CAST(start_date as varchar), 'n/a') as start_date,
  COALESCE(CAST(end_date as varchar), 'n/a') as end_date,
  COALESCE(name, 'no name') as name
FROM
  advertisement
JOIN product
  ON product_id = product.id
ORDER BY start_date
```

For each product with a non-NULL name, show its id and the number of advertisements associated with it. Sort the results by the id in descending order.

```sql
SELECT
  product.id,
  COUNT(product_id)
FROM product
LEFT JOIN advertisement
  ON product.id = product_id
WHERE name IS NOT NULL
GROUP BY product.id
ORDER BY product.id DESC
```

or each advertisement, show its id and the following sentence (as sentence):

The advertisement with id {id} for the product {name} was started on {date}.
Replace {id}, {name} and {date} with the proper columns. If there is no name, show the product id, if the product id is missing too, show 'no name'. If there is no started_date, show 'n/a'. Ignore products with missing price.

```sql
SELECT
  ad.id,
  'The advertisement with id ' || ad.id ||
  ' for the product ' || COALESCE(name, CAST(product.id as varchar), 'no name') || ' was started on ' ||
  COALESCE(CAST(start_date as varchar), 'n/a') || '.' as sentence
FROM advertisement as ad
JOIN product
  ON product_id = product.id
WHERE price IS NOT NULL
```

## Aggregate Functions

