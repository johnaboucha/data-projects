# Standard SQL Functions

## Pretty Printing

_item_

| id  | name | type |
| ----| ---- | ---- |
| 1   | WashPow(d)er | washing powder |
| … | … | … |

_slogan_

| id  | item_id | type | text | copywriter_id |
| ----| ---- | ---- | ---- | ---- |
| 1   | 1 | tv commercial | description | 3 |
| … | … | … | … | … |

_copywriter_

| id  | first_name | last_name |
| ----| ---- | ---- |
| 1   | Olivier | Norris |
| … | … | … |

Show each copywriter's first_name and last_name in one column separated with a single space. Name the column name.

```sql
SELECT
  first_name || ' ' || last_name as name
from copywriter
```

For each item, show the following sentence: 'ID X is Y.', where X is the id of the item and Y is its name. Name the column sentence.

```sql
SELECT
 'ID ' || id || ' is ' || name || '.' AS sentence 
FROM item
```

For each slogan, show the following sentence (name the column: sentence):

The copywriter of the slogan with ID X is Y Z. In this sentence, X is the id of the slogan, and Y and Z are the first and last name of the copywriter, respectively.

```sql
SELECT
  'The copywriter of the slogan with ID ' || slogan.id
  || ' is ' || first_name || ' ' || last_name || '.' AS sentence
FROM slogan
JOIN copywriter
  ON slogan.copywriter_id = copywriter.id
```

For every slogan, show its id, its text, and its length. Name the last column text_length.

```sql
SELECT
  id,
  text,
  length(text) AS text_length
FROM slogan
```

Show the IDs of all the items with a name longer than 8 characters. Show the length as the second column, name it name_length.

```sql
SELECT
  id,
  length(name) as name_length
FROM item
WHERE length(name) > 8
```

For every slogan of the type 'newspaper ad' or 'magazine ad', show the slogan id and its text in lowercase as text_lowercase.

```sql
SELECT
  id,
  lower(text) AS text_lowercase
FROM slogan
WHERE type = 'newspaper ad' OR type = 'magazine ad'
```

The lowercase fashion is now passé. Everybody started to use upper case. Show the id of each 'newspaper ad' or 'magazine ad' slogan together with its text in capital letters. Name the second column text_uppercase.

```sql
SELECT
  id,
  upper(text) AS text_uppercase
FROM slogan
WHERE type = 'newspaper ad' OR type = 'magazine ad'
```

Show the id of each item together with its old name (as old_name) and updated name (as new_name).

```sql
SELECT
  id,
  name AS old_name,
  initcap(name) AS new_name
FROM item
```

Show the id of the item whose name written in upper case is TRIPCARE.

```sql
SELECT
  id
FROM
  item
WHERE
  upper(name) = 'TRIPCARE'
```

For each slogan, print the following: 'X. Y', where X is the item name and Y is the text of the slogan. Name the column name_text.

```sql
SELECT
  name || '. ' || text AS name_text
FROM
  item
JOIN slogan
  ON item.id = slogan.item_id
```

Show the full name of each item and each name starting from the 3rd character. Name the second column name_substring.

```sql
SELECT
  name,
  substring(name, 3) AS name_substring
FROM item
```

For each slogan, show its text and the characters from 3 to 8 of that text. Name the second column text_substring.

```sql
SELECT
  text,
  substring(text, 3, 6) AS text_substring
FROM slogan
```

For slogan id = 1, show its original text and its text with the word 'Feel' replaced with 'Touch'. Name the second column changed_text.

```sql
SELECT
  text,
  replace(text, 'Feel', 'Touch') AS changed_text
FROM slogan
WHERE id = 1
```

For each slogan, show the item name and the slogan with all the periods (.) replaced by exclamation marks (!). Name the second column changed_text.

```sql
SELECT
  name,
  replace(text, '.', '!') AS changed_text
FROM item
JOIN slogan
ON item.id = slogan.item_id
```

Show the id of each item together with its name in capital letters and its type with an initial capital letter. The column names should be: id, name_uppercase, and type_initcap.

```sql
SELECT
  id,
  upper(name) AS name_uppercase,
  initcap(type) as type_initcap
FROM item
```

For each slogan with a text longer than 20 characters, show the text fragment from character 5 until character 20. Name the column text_substring.

```sql
SELECT
  substring(text, 5, 16) AS text_substring
FROM slogan
WHERE length(text) > 20
```

For each 'tv commercial' slogan, show the item name, the item type, and the text with each period (.) turned into three exclamation marks (!!!) – name this column changed_text.

```sql
SELECT
  name,
  item.type,
  replace(text, '.', '!!!') AS changed_text
FROM item
JOIN slogan
  ON item.id = slogan.item_id
WHERE slogan.type = 'tv commercial'
```

## Numeric Functions

_character_

| column name  | example value |
| ----| ---- |
| id   | 1 |
| player_id | 1 |
| name	 | Kav |
| level | 3 |
|	class	| wizard |
| account_balance	| 899.34 |
| hp	 | 100 |
| mp	 | 200 |
| strength | 12 |
| wisdom | 20 |
| stat_modifier | 1.05 |
| weight | 65 |
| height | 1.72 |

_player_

| id  | firstname | lastname |
| ----| ---- | ---- |
| 1   | Alan | Gilman |
| … | … | … |

For each character, display its:

- name
- level
- the sum of its hp and mp (name this column sum).

```sql
SELECT
  name,
  level,
  hp + mp AS sum
FROM character
```

For each character above the first level (level > 1), show the following text:

The account balance for {name} is {money}.

{name} is the name of the character and {money} is the current account_balance. Name the column sentence.

```sql
SELECT
  'The account balance for ' || name || ' is ' || account_balance || '.' AS sentence
FROM
  character
WHERE level > 1
```

For a character Mnah (name = 'Mnah'), select name, weight, height and the result of the following calculation: weight - height - weight + height (name the last column result). It should equal 0, right?

```sql
SELECT
  name,
  weight,
  height,
  weight - height - weight + height AS result
FROM
  character
WHERE
  name = 'Mnah'
```

For each character, show its name, level and the hp/mp ratio (column name ratio). Use casting to get a precise result.

```sql
SELECT
  name,
  level,
  CAST(hp as numeric)/mp AS ratio
FROM
  character
```

For each character, show its name and try to divide its mp by its wisdom. Filter out rows with 0 wisdom points. Name the second column ratio.

```sql
SELECT
  name,
  CAST(mp as numeric)/wisdom AS ratio
FROM
  character
WHERE wisdom != 0
```

For each character, show its name and calculate how many health points will be left if the player decides to sacrifice the maximum number of hp. Name the second column health_points.

```sql
SELECT
  name,
  mod(hp,7) AS health_points
FROM
  character
```

For each character, show its name, its account_balance and its account_balance rounded to the nearest integer (name this column rounded_account_balance). Notice how mathematical rounding is applied.

```sql
SELECT
  name,
  account_balance,
  round(account_balance) AS rounded_account_balance
FROM
  character
```

Show each character's name together with its original account_balance and account_balance rounded to a single decimal place (name this column rounded_account_balance).

```sql
SELECT
  name,
  account_balance,
  round(account_balance, 1) AS rounded_account_balance
FROM
  character
```

Let's check some rounding functions in the next few exercises. Show the character's name together with its weight, followed by the weight rounded up (name this column weight_rounded_up.

```sql
SELECT
  name,
  weight,
  ceil(weight) AS weight_rounded_up
FROM
  character
```

Show each character's name together with its account_balance followed by the account_balance rounded down. Name the last column floor.

```sql
SELECT
  name,
  account_balance,
  floor(account_balance) AS floor
FROM
  character
```

Show the character's name together with its stat_modifier, followed by the stat_modifier rounded with the function trunc(x,p) to a single decimal place. Name the last column trunc. Observe how it behaves for positive and negative numbers.

```sql
SELECT
  name,
  stat_modifier,
  trunc(stat_modifier,1) AS trunc
FROM
  character
```

For each character, show its stat_modifier and its absolute value (as absolute_value).

```sql
SELECT
  stat_modifier,
  abs(stat_modifier) AS absolute_value
FROM
  character
```

Show each character's name, strength and calculate the square root of strength for each of them. Name the last column square_root.

```sql
SELECT
  name,
  strength,
  sqrt(strength) AS square_root
FROM
  character
```

Calculate the BMI for every character in the table. BMI is calculated in the following way:

BMI = weight/height^2
​
Round the result to an integer. Show the characters name and the calculated BMI (column name bmi).

```sql
SELECT
  name,
  round(weight/(height*height)) AS bmi
FROM
  character
```
A healing potion costs 50 gold coins. For each character with an account balance of at least 100, calculate how many healing potions it can buy (column name potion_amount) and how much money it will have left as a result of that purchase (column change). Round the first column down to integers.

```sql
SELECT
  floor(account_balance / 50) AS potion_amount,
  mod(account_balance, 50) AS change
FROM
  character
WHERE
  account_balance >= 100
```

Each warrior (class = 'warrior') at level 3 or higher can perform a special attack whose damage is calculated as follows: its strength plus one fourth of its hp, and then multiplied by the absolute value of stat_modifier. Round this column to 2 decimal places.

Show the name and special attack damage (column damage) calculated for those characters which have the attack available.

```sql
SELECT
  name,
  round((strength + (hp/4.0)) * abs(stat_modifier),2) AS damage
FROM
  character
WHERE
  level >= 3 AND class = 'warrior'
```

For players with IDs equal to 1 and 5, show firstname and lastname together with the level, height rounded to two decimal places, and account_balance rounded down. The column names should be: firstname, lastname, level, height, and account_balance.

```sql
SELECT
  firstname,
  lastname,
  level,
  round(CAST(height as numeric),2) AS height,
  floor(account_balance) AS account_balance
FROM
  player
JOIN character
  ON player.id = character.player_id
WHERE player.id = 1 OR player.id = 5
```

## Date and Time Functions

_route_

| column name  | example value |
| ----| ---- |
| code   | PA2342 |
| from_airport | FRA |
| to_airport | WAW |
| distance | 558 |
| departure | 07:55:00 |
| arrival | 09:30:00 |

_aircraft_

| id  | model | produced | launched | withdrawn |
| ----| ---- | ---- | ---- | ---- |
| 1   | Airbus A321-200 | 2014-04-05 | 2014-06-10 07:55:00+00 | null |
| … | … | … | … | … |

_flight_

| id  | route_code | aircraft_id | date | delay |
| ----| ---- | ---- | ---- | ---- |
| 2   | PA2342 | 1 | 2016-01-11 | 15 |
| … | … | … | … | … |

Select the columns id, model and produced for those aircraft which were produced on April 6, 2008 or March 1, 2010.

```sql
SELECT
  id,
  model,
  produced
FROM
  aircraft
WHERE produced = '2008-04-06' or produced = '2010-03-01'
```

Select the columns id, model, produced for all the aircraft which were produced after December 31, 2012.

```sql
SELECT
  id,
  model,
  produced
FROM
  aircraft
WHERE
  produced > '2012-12-31'
```

Select the id and date of all flights which did NOT take place in 2015.

```sql
SELECT
  id,
  date
FROM
  flight
WHERE
  date NOT BETWEEN '2015-01-01' AND '2015-12-31'
```

Select the id and date of all flights in 2015 and sort them with the newest dates coming first.

```sql
SELECT
  id,
  date
FROM
  flight
WHERE date BETWEEN '2015-01-01' AND '2015-12-31'
ORDER BY date DESC
```

For each aircraft produced in 2010, show its id, production date and the distinctive codes of routes they operate on (rename the column to route_code).

```sql
SELECT DISTINCT
  aircraft.id,
  produced,
  route_code
FROM
  aircraft
JOIN
  flight
ON flight.aircraft_id = aircraft.id
WHERE produced BETWEEN '2010-01-01' AND '2010-12-31'
```

Show the code of the route with the scheduled arrival time of 9:30 AM.

```sql
SELECT code
FROM route
WHERE arrival = '09:30:00'
```

Show the code of all the routes which depart between 11:00 AM and 5:00 PM.

```
SELECT code
FROM route
WHERE departure BETWEEN '11:00:00' AND '17:00:00'
```

Show the code and arrival time of all the routes with arrival before 4:00 PM and order them by arrival with the earliest time coming first.

```sql
SELECT
  code,
  arrival
FROM
  route
WHERE arrival < '16:00:00'
ORDER BY arrival 
```

Show the average distance of all routes which depart after 8:00 AM. Name the column average.

```sql
SELECT
  AVG(distance) AS average
FROM route
WHERE departure > '8:00:00'
```

Identify the id and withdrawn timestamp for all the aircraft withdrawn on or after October 15, 2015. The column names should be id and withdrawn.

```sql
SELECT
  id,
  withdrawn
FROM
  aircraft
WHERE
  withdrawn >= '2015-10-15'
```

Find all the aircraft which were launched in 2015. Show the columns id and launched.

```sql
SELECT
  id,
  launched
FROM
  aircraft
WHERE launched BETWEEN '2015-01-01 00:00:00' AND '2015-12-31 23:59:59'
```

For all the aircraft launched in 2013 or 2014, show their id and launch date. Sort by the column launched from the newest to the oldest dates.

```sql
SELECT
  id,
  launched
FROM aircraft
WHERE launched BETWEEN '2013-01-01 00:00:00' AND '2014-12-31 23:59:59'
ORDER BY launched DESC
```

For each aircraft, show its id as aircraft_id and calculate the average distance (call it average) covered on all its routes. Only take into account those aircrafts which were launched before January 1, 2014 and took more than 1 flight.

```sql
SELECT
  aircraft.id AS aircraft_id,
  AVG(distance) AS average
FROM aircraft
JOIN flight
  ON aircraft.id = flight.aircraft_id
JOIN route
  ON flight.route_code = route.code
GROUP BY aircraft.id, launched
having launched < '2014-01-01 00:00:00'
  AND count(aircraft.id) > 1
```

Extract and show the month of each withdrawn date in the table aircraft (name the column month). Show the column withdrawn as the second column for reference.

```sql
SELECT EXTRACT (MONTH FROM withdrawn) AS month, withdrawn
FROM aircraft
```

For each route, show its code and the departure time in the following, changed format: hh.mm, where hh is the hour and mm the minutes. Name the second column time.

```sql
SELECT
  code,
  EXTRACT(HOUR FROM departure) || '.' || EXTRACT(MINUTE FROM departure) AS time
FROM route
```

For the route from Keflavik (KEF) to Gdansk (GDN), show the departure time from Keflavik in local time for Gdansk (Europe/Warsaw). Name the column local_time.

```sql
SELECT departure AT TIME ZONE 'Europe/Warsaw' as local_time
FROM route
WHERE from_airport = 'KEF' AND to_airport = 'GDN'
```

For each route with a distance greater than 600 km, show its code and its departure and arrival at the local time for Tokyo, Japan. Name the last two columns local_departure and local_arrival.

```sql
SELECT
  code,
  departure AT TIME ZONE 'Asia/Tokyo' AS local_departure,
  arrival AT TIME ZONE 'Asia/Tokyo' AS local_arrival
FROM
  route
WHERE distance > 600
```

PerfectAir decided to use the withdrawn aircraft (id = 5). Show its id, its original withdrawn date and the withdrawn date postponed by 1 year and 6 months. Name the last column changed_date.

```sql
SELECT
  id,
  withdrawn,
  withdrawn + INTERVAL '1-6' YEAR TO MONTH AS changed_date
FROM
  aircraft
WHERE id = 5
```

For plane with ID of 4, show the original launch timestamp and the timestamp of when its test period began (it lasted exactly 14 days, 8 hours, 41 minutes and 16 seconds). Name the second column test_date.

```sql
SELECT
  launched,
  launched - INTERVAL '14 8:41:16' DAY TO SECOND AS test_date
FROM aircraft
WHERE id = 4
```

PerfectAir has changed its schedules. All flights which depart after 1:00 PM have been delayed by 1 hour. Show the code of each route together with the new departure (as new_departure) and arrival times (as new_arrival).

```sql
SELECT
  code,
  departure + INTERVAL '1' HOUR AS new_departure,
  arrival + INTERVAL '1' HOUR AS new_arrival
FROM route
WHERE departure > '13:00:00'
```

Show the id and the model for all the aircraft produced in 2014 and 2015.

```sql
SELECT
  id,
  model
FROM aircraft
WHERE produced BETWEEN '2014-01-01'
  AND CAST('2014-01-01' AS date) + INTERVAL '1' YEAR
```

Count the number of flights performed in August 2015. Name the column flight_no.

```sql
SELECT
  COUNT(*) AS flight_no
FROM flight
WHERE date BETWEEN '2015-08-01'
  AND CAST('2015-08-01' as date) + INTERVAL '1' MONTH
```

Find the id of all the aircraft which were produced earlier than 3 months ago.

```sql
SELECT id
FROM aircraft
WHERE produced < CURRENT_DATE - INTERVAL '3' MONTH
```

Find the code of all the routes which normally depart within 3 hours from the current time.

```sql
SELECT code
FROM route
WHERE departure BETWEEN CURRENT_TIME
  AND CURRENT_TIME + INTERVAL '3' HOUR
```

Show the code, from_airport, and to_airport for all those routes which depart between 9:00 AM and 3:00 PM.

```sql
SELECT
  code,
  from_airport,
  to_airport
FROM route
WHERE departure BETWEEN '9:00:00' AND '15:00:00'
```

For all the flights which took place on July 11, 2015, show the from_airport, to_airport, and the model of the aircraft.

```sql
SELECT
  from_airport,
  to_airport,
  model
FROM flight
JOIN route
  ON flight.route_code = route.code
JOIN aircraft
  ON flight.aircraft_id = aircraft.id
WHERE date = '2015-07-11'
```

Calculate the average delay time for all the flights which took place in August (any year).

```sql
SELECT AVG(delay)
FROM flight
WHERE EXTRACT(MONTH FROM date) = 8
```

For each route, show its code together with the number of flights on that route and the average delay on that route. Only take into account flights older than 6 months. Don't show the routes with zero flights on it or NULL average delay. The column names should be: code, count, and avg.

```sql
SELECT
  code,
  COUNT(*) AS count,
  AVG(delay) AS avg
FROM
  route
JOIN flight
  ON flight.route_code = route.code
WHERE date < CURRENT_DATE - INTERVAL '6 MONTHS'
GROUP BY code
```

Group all the flights by the year. Show the year (as year) and calculate the average delay time for each of those years (as avg).

```sql
SELECT
  EXTRACT(YEAR from date) as year,
  AVG(delay) as avg
FROM flight
GROUP BY year
```

Count the number of distinct aircraft which were used during flights in August 2015. Name the column aircraft_no.

```sql
SELECT
  COUNT(DISTINCT model) AS aircraft_no
FROM aircraft
JOIN flight
  ON aircraft_id = aircraft.id
WHERE date BETWEEN '2015-08-01' AND '2015-08-31'
```