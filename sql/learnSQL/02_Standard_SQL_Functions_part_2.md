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

```
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