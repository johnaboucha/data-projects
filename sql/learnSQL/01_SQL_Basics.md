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

Find the sum of all salaries in the Marketing department in 2014. Remember to put the department name in inverted commas!

```
SELECT SUM(salary)
FROM employees
WHERE department = 'Marketing'
  AND year = 2014
```

Find the number of employees in each department in the year 2013. Show the department name together with the number of employees. Name the second column employees_no.

```
SELECT
  department,
  COUNT(*) as employees_no
FROM employees
WHERE year = 2013
GROUP BY department
```

Show all departments together with their lowest and highest salary in 2014.

```
SELECT
  department,
  MIN(salary),
  MAX(salary)
FROM employees
WHERE year = 2014
GROUP BY department
```

For each department find the average salary in 2015.

```
SELECT
  department,
  AVG(salary)
FROM employees
WHERE year = 2015
GROUP BY department
```

Find the average salary for each employee. Show the last name, the first name, and the average salary. Group the table by the last name and the first name.

```
SELECT
  last_name,
  first_name,
  salary
```

Find the average salary for each employee. Show the last name, the first name, and the average salary. Group the table by the last name and the first name.

```
SELECT
  last_name,
  first_name,
  AVG(salary)
FROM employees
GROUP BY last_name, first_name
```

Find such employees who (have) spent more than 2 years in the company. Select their last name and first name together with the number of years worked (name this column years).

```
SELECT
  last_name,
  first_name,
  COUNT(DISTINCT year) as years
FROM employees
GROUP BY last_name, first_name
HAVING COUNT(years) > 2
```

Find such departments where the average salary in 2012 was higher than $3,000. Show the department name with the average salary.

```
SELECT
  department,
  AVG(salary)
FROM employees
WHERE year = 2012
GROUP BY department
HAVING AVG(salary) > 3000
```

Sort the employees according to their summary salaries. Highest values should appear first. Show the last name, the first name, and the sum.

```
SELECT
  last_name,
  first_name,
  SUM(salary)
FROM employees
GROUP BY last_name, first_name
ORDER BY SUM(salary) DESC
```

Show the columns last_name and first_name from the table employees together with each person's average salary and the number of years they (have) worked in the company.

Use the following aliases: average_salary for each person's average salary and years_worked for the number of years worked in the company. Show only such employees who (have) spent more than 2 years in the company. Order the results according to the average salary in the descending order.

```
SELECT
  last_name,
  first_name,
  AVG(salary) as average_salary,
  COUNT(DISTINCT year) as years_worked
FROM employees
GROUP BY last_name, first_name
HAVING years_worked > 2
ORDER BY average_salary DESC
```

## Student and room tables

_student_

| id | name | room_id |
| ---- | ----| ---- |
| 1 | Jack Pearson | 8 |
| … | … | … |

_room_

| id | room | beds | floor |
| ---- | ----| ---- | ---- |
| 1 | 101 | 2 | 1 |
| … | … | … | … |

_equipment_

| id | name | room_id |
| ---- | ----| ---- |
| 1 | kettle | 4 |
| … | … | … |

Get all the data from the table student.

```
SELECT * FROM student
```

Join the two tables: student and room so that each student is shown together with the room they live in. Select all columns.

```
SELECT *
FROM student
JOIN room ON
  student.room_id = room.id
```

Join the student and room tables so that each student is shown together with the room they live in.

Select the name of the student and room number.

```
SELECT
  name,
  room_number
FROM student
JOIN room ON
  student.room_id = room.id
```

Now, use the full name INNER JOIN to join the room and equipment tables, so that each piece of equipment is shown together with its room and other relevant columns.

```
SELECT
  room_id,
  room_number,
  beds,
  floor,
  equipment.id as equipment_id,
  equipment.name
FROM room
INNER JOIN equipment ON
  room.id = equipment.room_id
```

Show all rows from the table student. If a student is assigned to a room, show the room data as well.

```
SELECT *
FROM student
LEFT JOIN room ON
  student.room_id = room.id
```

Select all pieces of equipment together with the room they are assigned to. Show each piece of equipment even if it isn't assigned to a room. Select all available columns.

```
SELECT *
FROM equipment
LEFT JOIN room ON
  equipment.room_id = room.id
```

For each student show their data with the data of the room they live in. Show also rooms with no students assigned. Use a RIGHT JOIN.

```
SELECT *
FROM student
RIGHT JOIN room ON
  student.room_id = room.id
```

Use the full name RIGHT OUTER JOIN to show all the kettles together with their room data (even if there is no room assigned).

```
SELECT *
FROM room
RIGHT OUTER JOIN equipment ON
  equipment.room_id = room.id
WHERE name = 'kettle'
```

Use NATURAL JOIN on the tables student and room.

```
SELECT *
FROM student
NATURAL JOIN room
```

Use INNER JOIN on the tables room and equipment so that all pieces of equipment are shown with their room data. Use table aliases r and e. Select the columns id and name from the table equipment, as well as room_number and beds from the table room.

```
SELECT 
  e.id,
  e.name,
  r.room_number,
  r.beds
FROM room as r
INNER JOIN equipment as e ON
  r.id = e.room_id
```

Use self-joining to show all the columns for the student Jack Pearson together with all the columns for each student living with him in the same room.

```
SELECT *
FROM student as jack_pearson
JOIN student as roommate
  ON jack_pearson.room_id = roommate.room_id
WHERE jack_pearson.name = 'Jack Pearson'
  AND jack_pearson.id IS NOT roommate.id
```

For each room with 2 beds where there actually are 2 students, we want to show one row which contains the following columns:

- the name of the first student.
- the name of the second student.
- the room number.

Don't change any column names. Each pair of students should only be shown once. The student whose name comes first in the alphabet should be shown first.

```
SELECT
  s1.name,
  s2.name,
  room_number
FROM student as s1
JOIN student as s2
  ON s1.room_id = s2.room_id
JOIN room
  ON s1.room_id = room.id
WHERE s1.name < s2.name AND beds = 2
```

## Geo tables

_trip_

| id | city_id | days | price |
| ---- | ----| ---- | ---- |
| 1 | 1 | 3 | 1200 |
| … | … | … | … |

_mountain_

| id | name | height | country_id |
| ---- | ----| ---- | ---- |
| 1 | Mont Blanc | 4808 | 1 |
| … | … | … | … |

_hiking_trip_

| id | mountain_id | days | price | length | difficulty |
| ---- | ----| ---- | ---- | ---- | ---- |
| 1 | 1 | 3 | 1000 | 30 | 3 |
| … | … | … | … | … | … |

_country_

| id | name | population | area |
| ---- | ----| ---- | ---- |
| 1 | France | 66600000 | 640680 |
| … | … | … | … |

_city_

| id | name | country_id | population | area | rating |
| ---- | ----| ---- | ---- | ---- | ---- |
| 1 | Paris | 1 | 2243000 | 102 | 5 |
| … | … | … | … | … | … |

Show all information about all cities which have the same area as Paris.

```
SELECT *
FROM city
WHERE area = (
  SELECT area
  FROM city
  WHERE name = 'Paris'
)
```

Find the names of all cities which have a population lower than Madrid.

```
SELECT name
FROM city
WHERE population < (
  SELECT population
  FROM city
  WHERE name = 'Madrid'
)
```

Find all information about trips whose price is higher than the average.

```
SELECT *
FROM trip
WHERE price > (
  SELECT AVG(price)
  FROM trip
)
```

Find all information about hiking trips with difficulty 1, 2, or 3.

```
SELECT *
FROM hiking_trip
WHERE difficulty IN (1,2,3)
```

Find all information about all trips in cities whose area is greater than 100.

```
SELECT *
FROM trip
WHERE city_id IN (
  SELECT id
  FROM city
  WHERE area > 100
)
```

Find all information about the cities which are less populated than all countries in the database.

```
SELECT *
FROM city
WHERE population < ALL (
  SELECT population
  FROM country
)
```

Find all information about all the city trips which have the same price as any hiking trip.

```
SELECT *
FROM trip
WHERE price = ANY (
  SELECT price
  FROM hiking_trip
)
```

Find all information about each country whose population is equal to or smaller than the population of the least populated city in that specific country.

```
SELECT *
FROM country
WHERE population <= (
  SELECT MIN(population)
  FROM city
  WHERE country.id = city.country_id
)
```

Find all information about cities with a rating higher than the average rating for all cities in that specific country.

```
SELECT *
FROM city AS avg_city
WHERE rating > (
  SELECT AVG(rating)
  FROM city
  WHERE city.country_id = avg_city.country_id
)
```

Show all information about all trips to cities which have a rating lower than 4.

```
SELECT *
FROM trip
WHERE city_id IN (
  SELECT id
  FROM city
  WHERE rating < 4
)
```

Select all countries where there is at least one mountain.

```
SELECT *
FROM country
WHERE EXISTS (
  SELECT *
  FROM mountain
  WHERE country_id = country.id
)
```

Select all mountains with no hiking trips to them.

```
SELECT *
FROM mountain
WHERE NOT EXISTS (
  SELECT *
  FROM country
  WHERE country_id = country.id
)
```

Select the hiking trip with the longest distance (column length) for every mountain.

```
SELECT *
FROM hiking_trip AS main_trip
WHERE length >= ALL (
  SELECT length
  FROM hiking_trip AS sub_trip
  WHERE main_trip.mountain_id = sub_trip.mountain_id
)
```

Select those trips which last shorter than any hiking_trip with the same price.

```
SELECT *
FROM trip
WHERE days < ANY (
  SELECT days
  FROM hiking_trip
  WHERE trip.price = hiking_trip.price
)
```

Show mountains together with their countries. The countries must have at least 50,000 residents.

```
SELECT *
FROM mountain, (
  SELECT *
  FROM country
  WHERE population > 50000
) AS country
WHERE mountain.country_id = country.id
```

Show hiking trips together with their mountains. The mountains must be at least 3,000 high. Select only the columns length and height.

```
SELECT
  length,
  height
FROM hiking_trip, (
  SELECT *
  FROM mountain
  WHERE height > 3000
) as mountain
WHERE mountain.id = hiking_trip.mountain_id
```

Show each mountain name together with the number of hiking trips to that mountain (name the column count).

```
SELECT
  name,
  (
    SELECT COUNT(*)
    FROM hiking_trip
    WHERE mountain_id = mountain.id
  ) AS count
FROM mountain
```

## Athlete tables

_skating_

| id | person | country | year | place |
| ---- | ----| ---- | ---- |
| 1 | Clara Hughes | Canada | 2006 | 1 |
| … | … | … | … | … |

_cycling_

| id | person | country | year | place |
| ---- | ----| ---- | ---- |
| 1 | Clara Hughes | Canada | 1996 | 3 |
| … | … | … | … | … |

Show all the medals for the period between 2010 and 2014 for skating and cycling. Use the UNION keyword.

```
SELECT *
FROM skating
WHERE year BETWEEN 2010 AND 2014
UNION
SELECT *
FROM cycling
WHERE year BETWEEN 2010 AND 2014
```

Show all countries which have medals in cycling or skating. Use a union. Don't remove duplicates.

```
SELECT country
FROM cycling
UNION ALL
SELECT country
FROM skating
```

Find names of each person who has medals both in cycling and in skating.

```
SELECT person
FROM skating
INTERSECT
SELECT person
FROM cycling
```

Find all the countries which have a medal in cycling but not in skating.

```
SELECT country
FROM cycling
EXCEPT
SELECT country
FROM skating
```

## Horoscope table

| id | year | sign | content |
| ---- | ----| ---- |
| 1 | 2007 | sign | blah, blah, blah… |
| … | … | … | … |


