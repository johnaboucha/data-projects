# SQL Basics

An intro course on the basics of SQL.

## Car Table

Car table fields:


| VIN | BRAND | MODEL | PRICE | PRODUCTION_YEAR |
| ---- | ----| ---- | ---- | ---- |
| LJCPCBLCX14500264 | FORD | Focus | 8000.00 | 2005 |
| … | … | … | … | … |

Select all data from the car table.

```
SELECT * FROM CAR;
```

Select brand names from the car table.

```
SELECT BRAND FROM CAR;
```

Select model and price from table car.

```
SELECT
  MODEL, PRICE
FROM CAR
```

Select all columns for those cars which were produced (column production_year) in 1999.

```
SELECT *
FROM CAR
WHERE PRODUCTION_YEAR = 1999
```

Select all columns for all cars with price higher than $10,000.

```
SELECT *
FROM CAR
WHERE PRICE > 10000
```

Select all columns for those cars which weren't produced in 1999.

```
SELECT *
FROM CAR
WHERE PRODUCTION_YEAR != 1999
```

Select brand, model and production year of all cars cheaper than or equal to $11,300.

```
SELECT BRAND, MODEL, PRODUCTION_YEAR
FROM CAR
WHERE PRICE <= 11300
```

Select vins of all cars which were produced before 2005 or whose price is below $10,000.

```
SELECT VIN
FROM CAR
WHERE PRODUCTION_YEAR < 2005 OR PRICE < 10000
```

Select vins of all cars which were produced after 1999 and are cheaper than $7,000.

```
SELECT VIN
FROM CAR
WHERE PRODUCTION_YEAR > 1999 AND PRICE < 7000
```

Select vin, brand, and model of all cars which were produced between 1995 and 2005.

```
SELECT
  VIN, BRAND, MODEL
FROM CAR
WHERE PRODUCTION_YEAR BETWEEN 1995 AND 2005
```

Select vin, brand, and model of all cars except for those produced between 1995 and 2005.

```
SELECT
  VIN, BRAND, MODEL
FROM CAR
WHERE PRODUCTION_YEAR NOT BETWEEN 1995 AND 2005
```

Select the vin of all cars which were produced before 1999 or after 2005 and whose price is lower than $4,000 or greater than $10,000.

```
SELECT VIN
FROM CAR
WHERE (PRODUCTION_YEAR NOT BETWEEN 1999 AND 2005)
  AND (PRICE < 4000 OR PRICE > 10000)
```

Select all columns of all Ford cars from the table.

```
SELECT *
FROM CAR
WHERE BRAND = 'Ford'
```

Select vin, brand, and model of all cars whose brand begins with an F.

```
SELECT
  VIN, BRAND, MODEL
FROM CAR
WHERE BRAND LIKE 'F%'
```

Select vin of all cars whose model ends with an s.

```
SELECT VIN
FROM CAR
WHERE MODEL LIKE '%s'
```

Select all columns for cars which brand matches 'Volk_wagen'.

```
SELECT *
FROM CAR
WHERE BRAND LIKE 'Volk_wagen'
```

Select all columns for each car whose price column isn't a NULL value.

```
SELECT *
FROM CAR
WHERE PRICE IS NOT NULL
```

Select all columns for each car whose price is unknown (NULL).

```
SELECT *
FROM CAR
WHERE PRICE IS NULL
```

Select all columns for cars whose price column is greater than or equal to zero.

```
SELECT *
FROM CAR
WHERE PRICE > 0
```

Select all columns for cars with a tax amount over $2000. The tax amount for all cars is 20% of their price. Multiply the price by 0.2 to get the tax amount.

```
SELECT *
FROM CAR
WHERE PRICE * 0.2 > 2000
```

Select all columns of those cars that:

- were produced between 1999 and 2005,
- are not Volkswagens,
- have a model that begins with either 'P' or 'F'
- have their price set.

```
SELECT *
FROM CAR
WHERE (PRODUCTION_YEAR BETWEEN 1999 AND 2005)
  AND (BRAND IS NOT 'Volkswagen')
  AND (MODEL LIKE 'P%' OR MODEL LIKE 'F%')
  AND (PRICE > 0)
```

## Movie and Director tables

*_movie_*

| ID | TITLE | PRODUCTION_YEAR | DIRECTOR_ID |
| ---- | ----| ---- | ---- |
| 1 | Pyscho | 1960 | 1 |
| … | … | … | … |

*_director_*

| ID | NAME | BIRTH_YEAR |
| ---- | ----| ---- |
| 1 | Alfred Hitchcock | 1899 |
| … | … | … |