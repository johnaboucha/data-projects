# Window Functions - part 1

## Over()

_employee_

| id | first_name | last_name | department_id | salary | years_worked |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 1 | Diane | Turner | 1 | 5330 | 4 |
| … | … | … | … | … | … |

_department_

| id | name |
| ---- | ---- |
| 1 | IT |
| … | … |

_purchase_

| id | department_id | item | price |
| ---- | ---- | ---- | ---- |
| 1 | 4 | monitor | 531 |
| … | … | … | … |

For each employee, find their first name, last name, salary and the sum of all salaries in the company.

Note that the last column is an aggregated column, even though you're not using a GROUP BY.

```sql
SELECT
  first_name,
  last_name,
  salary,
  SUM(salary) OVER()
FROM employee
```

For each item in the purchase table, select its name (column item), price and the average price of all items.

```sql
SELECT
  item,
  price,
  AVG(price) OVER()
FROM purchase
```

For each employee in table employee, select first and last name, years_worked, average of years spent in the company by all employees, and the difference between the years_worked and the average as difference.

```sql
SELECT
  first_name,
  last_name,
  years_worked,
  AVG(years_worked) OVER(),
  years_worked - AVG(years_worked) OVER() as difference
FROM employee
```

For all employees from department with department_id = 3, show their:

- first_name,
- last_name,
- salary,
- the difference of their salary to the average of all salaries in that department as difference.

```sql
SELECT
  first_name,
  last_name,
  salary,
  salary - AVG(salary) OVER() as difference
FROM employee
WHERE department_id = 3
```

For each employee that earns more than 4000, show their first_name, last_name, salary and the number of all employees who earn more than 4000.

```sql
SELECT
  first_name,
  last_name,
  salary,
  COUNT(id) OVER()
FROM employee
WHERE salary > 4000
```

For each purchase with department_id = 3, show its:

- id,
- department_id,
- item,
- price,
- max – the maximum price from all purchases in this department,
- the difference between the maximum price and the price.

```sql
SELECT
  id,
  department_id,
  item,
  price,
  MAX(price) OVER(),
  MAX(price) OVER() - price as difference
FROM purchase
WHERE department_id = 3
```

For each purchase from any department, show its id, item, price, average price and the sum of all prices in that table.

```sql
SELECT
  id,
  item,
  price,
  AVG(price) OVER(),
  SUM(price) OVER()
FROM purchase
```

Show the first_name, last_name and salary of every person who works in departments with id 1, 2 or 3, along with the average salary calculated in those three departments.

```sql
SELECT
  first_name,
  last_name,
  salary,
  AVG(salary) OVER()
FROM employee
WHERE department_id IN (1,2,3)
```

For each employee from department with id 1, 3 or 5, show their first name, last name, years_worked and the average number of years_worked in those departments.

```sql
SELECT
  first_name,
  last_name,
  years_worked,
  AVG(years_worked) OVER()
FROM employee
WHERE department_id IN (1,3,5)
```

For each purchase, show its:

- id,
- the name of the department,
- the item,
- the price,
- the minimum price from all the rows in the query result,
- the difference between the price and the minimum price.

```sql
SELECT
  purchase.id,
  name,
  item,
  price,
  MIN(price) OVER(),
  price - MIN(price) OVER()
FROM purchase
JOIN department
  ON purchase.department_id = department.id
```

## OVER(Partition By)

_train_

| column name | example value |
| ---- | ---- |
| id | 1 |
| model | InterCity 100 |
| max_speed | 160 |
| production_year | 2000 |
| first_class_places | 30 |
| second_class_places | 230 |

_route_

| column name | example value |
| ---- | ---- |
| id | 1 |
| name | Manchester Express |
| from_city | Sheffield |
| to_city | Manchester |
| distance | 30 |

_journey_

| column name | example value |
| ---- | ---- |
| id | 1 |
| train_id | 1 |
| route_id | 1 |
| date | 2016-01-03 |

_journey_id_

| column name | example value |
| ---- | ---- |
| id | 1 |
| price | 100 |
| class | 1 |
| journey_id | 23 |

Show the id of each journey, its date and the number of journeys that took place on that date.

```sql
SELECT
  id,
  date,
  COUNT(id) OVER(PARTITION BY date)
FROM journey
```

Show id, model, first_class_places, second_class_places, and the number of trains of each model with more than 30 first class places and more than 180 second class places.

```sql
SELECT
  id,
  model,
  first_class_places,
  second_class_places,
  COUNT(id) OVER(PARTITION BY model)
FROM train
WHERE first_class_places > 30 AND second_class_places > 180
```

Show the id of each journey, the date on which it took place, the model of the train that was used, the max_speed of that train and the highest max_speed from all the trains that ever went on the same route on the same day.

```sql
SELECT
  journey.id,
  date,
  model,
  max_speed,
  MAX(max_speed) OVER(PARTITION BY route_id, date)
FROM journey
JOIN train
  ON journey.train_id = train.id
```

For each journey, show its id, the production_year of the train on that journey, the number of journeys the train took and the number of journeys on the same route.

```sql
SELECT
  journey.id,
  production_year,
  COUNT(journey.id) OVER(),
  COUNT(journey.id) OVER(PARTITION BY route_id)
FROM journey
JOIN route
  ON journey.route_id = route.id
JOIN train
  ON journey.train_id = train.id
```

For each ticket, show its id, price, date of its journey, the average price of tickets sold on that day and the number of tickets sold on that day. Exclude journeys with train_id = 5.

```sql
SELECT
  ticket.id,
  price,
  date,
  AVG(price) OVER(PARTITION BY date),
  COUNT(ticket.id) OVER(PARTITION BY date)
FROM ticket
JOIN journey
  ON ticket.journey_id = journey.id
WHERE train_id != 5
```

For each ticket, show its id, price and, the column named ratio. The ratio is the ticket price to the sum of all ticket prices purchased on the same journey.

```sql
SELECT
  id,
  price,
  price / CAST(SUM(price) OVER(PARTITION BY journey_id) as decimal) as ratio
FROM ticket
```

## Ranking Functions

_game_

| column name | example value |
| ---- | ---- |
| id | 1 |
| name | Go Bunny |
| platform | iOS |
| genre | action |
| editor_rating | 5 |
| size | 101 |
| released | 2015-05-01 |
| updated | 2015-07-13 |

_purchase_

| column name | example value |
| ---- | ---- |
| id | 1 |
| game_id | 7 |
| price | 15.99 |
| date | 2016-03-07 |

For each game, show name, genre, date of update and its rank. The rank should be created with RANK() and take into account the date of update.

```sql
SELECT
  name,
  genre,
  updated,
  RANK() OVER(ORDER BY updated)
FROM game
```

Use DENSE_RANK() and for each game, show name, size and the rank in terms of its size.

```sql
SELECT
  name,
  size,
  DENSE_RANK() OVER(ORDER BY size)
FROM game
```

Use ROW_NUMBER() and for each game, show their name, date of release and the rank based on the date of release.

```sql
SELECT
  name,
  released,
  ROW_NUMBER() OVER(ORDER BY released)
FROM game
```

For each game, show its name, genre and date of release. In the next three columns, show RANK(), DENSE_RANK() and ROW_NUMBER() sorted by the date of release.

```sql
SELECT
  name,
  genre,
  released,
  RANK() OVER(ORDER BY released),
  DENSE_RANK() OVER(ORDER BY released),
  ROW_NUMBER() OVER(ORDER BY released)
FROM game
```

Let's use DENSE_RANK() to show the latest games from our studio. For each game, show its name, genre, date of release and DENSE_RANK() in the descending order.

```sql
SELECT
  name,
  genre,
  released,
  DENSE_RANK() OVER(ORDER BY released DESC)
FROM game
```

We want to find games which were both recently released and recently updated. For each game, show name, date of release and last update date, as well as their rank: use ROW_NUMBER(), sort by release date and then by update date, both in the descending order.

```sql
SELECT
  name,
  released,
  updated,
  ROW_NUMBER() OVER(ORDER BY released DESC, updated DESC)
FROM game
```

For each game find its name, genre, its rank by size. Order the games by date of release with newest games coming first.

```sql
SELECT
  name,
  genre,
  RANK() OVER(ORDER BY size)
FROM game
ORDER BY released DESC
```

For each purchase, find the name of the game, the price, and the date of the purchase. Give purchases consecutive numbers by date when the purchase happened, so that the latest purchase gets number 1. Order the result by editor's rating of the game.

```sql
SELECT
  name,
  price,
  date,
  ROW_NUMBER() OVER(ORDER BY date DESC)
FROM purchase
JOIN game
  ON purchase.game_id = game.id
ORDER BY editor_rating
```

We want to divide games into 4 groups with regard to their size, with biggest games coming first. For each game, show its name, genre, size and the group it belongs to.

```sql
SELECT
  name,
  genre,
  size,
  NTILE(4) OVER(ORDER BY size DESC)
FROM game
```

Split the games into 5 groups based on their date of last update. The most recently updated games should come first. For each of them, show the name, genre, date of update and the group they were assigned to. In the result, notice how many items the groups have (varying value).

```sql
SELECT
  name,
  genre,
  updated,
  NTILE(5) OVER(ORDER BY updated DESC)
FROM game
```

Find the name, genre and size of the smallest game in our studio.

Remember the steps:

1. Create the ranking query so that the smallest game gets rank 1.
1. Use WITH to select rows with rank 1.

```sql
WITH ranking AS (
SELECT
  name,
  genre,
  size,
  RANK() OVER(ORDER BY size) as rank_size
FROM game
)

SELECT
  name,
  genre,
  size
FROM ranking
WHERE rank_size = 1
```

Show the name, platform and update date of the second most recently updated game.

```sql
WITH ranking as (
  SELECT
  name,
  platform,
  updated,
  RANK() OVER(ORDER BY updated DESC) updated_rank
FROM game
  )
  
SELECT
  name,
  platform,
  updated
FROM ranking
WHERE updated_rank = 2
```