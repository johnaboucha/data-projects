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

```
SELECT name
FROM product
WHERE category IS NULL
```

Select the name for all the products with a price different than 299.99. Include NULLs!

```
SELECT name
FROM product
WHERE price != 299.99
  OR price IS NULL
```

Select the name for all products together with their categories and types. Exclude rows with category 'kitchen' and those rows which have no category set.

```
SELECT
  name,
  category,
  type
FROM product
WHERE category != 'kitchen'
```

Mr Amund released a certain series of products on April 30, 2014. Now, he would like to get all the names, categories and types of those products which were introduced any time later or which will be introduced in the future (i.e., they don't have a launch_date yet). Write the proper query.

```
SELECT
  name,
  category,
  type
FROM product
WHERE launch_date > '2014-04-30'
  OR launch_date IS NULL
```