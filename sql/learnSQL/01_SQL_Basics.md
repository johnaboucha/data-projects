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

_movie_

| ID | TITLE | PRODUCTION_YEAR | DIRECTOR_ID |
| ---- | ----| ---- | ---- |
| 1 | Pyscho | 1960 | 1 |
| … | … | … | … |

_director_

| ID | NAME | BIRTH_YEAR |
| ---- | ----| ---- |
| 1 | Alfred Hitchcock | 1899 |
| … | … | … |

Get all the data from two tables: movie and director.

```
SELECT *
FROM movie, director
```

Select all columns from tables movie and director in such a way that a movie is shown together with its director.

```
SELECT *
FROM movie, director
WHERE movie.director_id = director.id
```

Use the new construction JOIN ... ON to join rows from the tables movie and director in such a way that a movie is shown together with its director. Display all columns from both tables.

```
SELECT *
FROM movie
JOIN director
  ON movie.director_id = director.id
```

Select director name and movie title from tables movie and director in such a way that a movie is shown together with its director.

```
SELECT
  movie.title,
  director.name
FROM
  movie
JOIN director ON
  movie.director_id = director.id
```

Select director name and movie title from the movie and director tables in such a way that a movie is shown together with its director. Don't write table names in the SELECT clause.

```
SELECT
  name,
  title
FROM
  movie
JOIN director ON
  movie.director_id = director.id
```

In this exercise, show the title column as movie_title. We wrote the query from the previous exercise in the template, so you just have to add a proper alias.

```
SELECT
  title as movie_title,
  name
FROM movie
JOIN director
  ON director_id = director.id;
```

Select all columns from tables movie and director in such a way that a movie is shown together with its director. Select only those movies which were made after 2000. In the joining condition, let the first table be movie and the second table be director.

```
SELECT *
FROM movie
JOIN director ON
  movie.director_id = director.id
WHERE movie.production_year > 2000
```

Select all columns from tables movie and director in such a way that a movie is shown together with its director. Select only those movies which were directed by Steven Spielberg.

```
SELECT *
FROM movie
JOIN director ON
  movie.director_id = director.id
WHERE director.name = 'Steven Spielberg'
```

Select the title and production_year columns from the movie table, and the name and birth_year columns from the director table in such a way that a movie is shown together with its director.

Show the column birth_year as born_in. Select only those movies which were filmed when their director was younger than 40 (i.e. the difference between production_year and birth_year must be less than 40).

```
SELECT
  title,
  production_year,
  director.name,
  birth_year as born_in
FROM
  movie
JOIN director ON
  movie.director_id = director.id
WHERE production_year - birth_year < 40
```

Select the id, title, and production_year columns from the movie table, and the name and birth_year columns from the director table in such a way that a movie is shown together with its director. Show the column birth_year as born_in and the column production_year as produced_in.

Select only those movies:

- whose title contains a letter 'a' and which were filmed after 2000, **or**
- whose director was born between 1945 and 1995.

```
SELECT
  movie.id,
  title,
  production_year as produced_in,
  director.name,
  birth_year as born_in
FROM
  movie
JOIN director ON
  movie.director_id = director.id
WHERE (title LIKE '%a%' AND production_year > 2000)
   OR (birth_year BETWEEN 1945 AND 1995)
```

## Employees table

| department | first_name | last_name | year | salary | position  |
| ---- | ----| ---- | ---- | ---- | ---- |
| IT | Olivia | Pearson | 2011 | 3000 | Trainee |
| … | … | … | … | … | … |

Select all columns from the table employees and sort them according to the salary.

```
SELECT *
FROM employees
ORDER BY salary
```

Select only the rows related to the year 2011 from the table employees. Sort them by salary.

```
SELECT *
FROM employees
WHERE year = 2011
ORDER BY salary
```

Select all rows from the table employees and sort them in the descending order by the column last_name.

```
SELECT *
FROM employees
ORDER BY last_name DESC
```

Select all rows from the table employees and sort them in the ascending order by the department and then in the descending order by the salary.

```
SELECT *
FROM employees
ORDER BY department ASC, salary DESC
```

Select the column year for all rows in the table employees. Then examine the result carefully.

```
SELECT year
FROM employees
```

Select the column year from the table employees in such a way that each year is only shown once.

```
SELECT DISTINCT year
FROM employees
```

Select the columns department and position from the table employees and eliminate duplicates.

```
SELECT DISTINCT
  department,
  position
FROM employees
```

Count all rows in the table employees.

```
SELECT COUNT(*)
FROM employees
```

Check how many non-NULL values in the column position there are in the table employees. Name the column non_null_no.

```
SELECT COUNT(position) as non_null_no
FROM employees
```

Count how many different positions there are in the table employees. Name the column distinct_positions.

```
SELECT COUNT(DISTINCT position) as distinct_positions
FROM employees
```

Select the highest salary from the table employees.

```
SELECT MAX(salary)
FROM employees
```

Find the average salary in the table employees for the year 2013.

```
SELECT AVG(salary)
FROM employees
WHERE year = 2013
```