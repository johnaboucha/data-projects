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

```
SELECT
  first_name || ' ' || last_name as name
from copywriter
```

For each item, show the following sentence: 'ID X is Y.', where X is the id of the item and Y is its name. Name the column sentence.

```
SELECT
 'ID ' || id || ' is ' || name || '.' AS sentence 
FROM item
```

For each slogan, show the following sentence (name the column: sentence):

The copywriter of the slogan with ID X is Y Z. In this sentence, X is the id of the slogan, and Y and Z are the first and last name of the copywriter, respectively.

```
SELECT
  'The copywriter of the slogan with ID ' || slogan.id
  || ' is ' || first_name || ' ' || last_name || '.' AS sentence
FROM slogan
JOIN copywriter
  ON slogan.copywriter_id = copywriter.id
```

For every slogan, show its id, its text, and its length. Name the last column text_length.

```
SELECT
  id,
  text,
  length(text) AS text_length
FROM slogan
```

Show the IDs of all the items with a name longer than 8 characters. Show the length as the second column, name it name_length.

```
SELECT
  id,
  length(name) as name_length
FROM item
WHERE length(name) > 8
```

For every slogan of the type 'newspaper ad' or 'magazine ad', show the slogan id and its text in lowercase as text_lowercase.

```
SELECT
  id,
  lower(text) AS text_lowercase
FROM slogan
WHERE type = 'newspaper ad' OR type = 'magazine ad'
```

The lowercase fashion is now passé. Everybody started to use upper case. Show the id of each 'newspaper ad' or 'magazine ad' slogan together with its text in capital letters. Name the second column text_uppercase.

```
SELECT
  id,
  upper(text) AS text_uppercase
FROM slogan
WHERE type = 'newspaper ad' OR type = 'magazine ad'
```

Show the id of each item together with its old name (as old_name) and updated name (as new_name).

```
SELECT
  id,
  name AS old_name,
  initcap(name) AS new_name
FROM item
```

Show the id of the item whose name written in upper case is TRIPCARE.

```
SELECT
  id
FROM
  item
WHERE
  upper(name) = 'TRIPCARE'
```

For each slogan, print the following: 'X. Y', where X is the item name and Y is the text of the slogan. Name the column name_text.

```
SELECT
  name || '. ' || text AS name_text
FROM
  item
JOIN slogan
  ON item.id = slogan.item_id
```

Show the full name of each item and each name starting from the 3rd character. Name the second column name_substring.

```
SELECT
  name,
  substring(name, 3) AS name_substring
FROM item
```

For each slogan, show its text and the characters from 3 to 8 of that text. Name the second column text_substring.

```
SELECT
  text,
  substring(text, 3, 6) AS text_substring
FROM slogan
```

For slogan id = 1, show its original text and its text with the word 'Feel' replaced with 'Touch'. Name the second column changed_text.

```
SELECT
  text,
  replace(text, 'Feel', 'Touch') AS changed_text
FROM slogan
WHERE id = 1
```

For each slogan, show the item name and the slogan with all the periods (.) replaced by exclamation marks (!). Name the second column changed_text.

```
SELECT
  name,
  replace(text, '.', '!') AS changed_text
FROM item
JOIN slogan
ON item.id = slogan.item_id
```

Show the id of each item together with its name in capital letters and its type with an initial capital letter. The column names should be: id, name_uppercase, and type_initcap.

```
SELECT
  id,
  upper(name) AS name_uppercase,
  initcap(type) as type_initcap
FROM item
```

For each slogan with a text longer than 20 characters, show the text fragment from character 5 until character 20. Name the column text_substring.

```
SELECT
  substring(text, 5, 16) AS text_substring
FROM slogan
WHERE length(text) > 20
```

For each 'tv commercial' slogan, show the item name, the item type, and the text with each period (.) turned into three exclamation marks (!!!) – name this column changed_text.

```
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

```
SELECT
  name,
  level,
  hp + mp AS sum
FROM character
```

For each character above the first level (level > 1), show the following text:

The account balance for {name} is {money}.

{name} is the name of the character and {money} is the current account_balance. Name the column sentence.

```
SELECT
  'The account balance for ' || name || ' is ' || account_balance || '.' AS sentence
FROM
  character
WHERE level > 1
```

For a character Mnah (name = 'Mnah'), select name, weight, height and the result of the following calculation: weight - height - weight + height (name the last column result). It should equal 0, right?

```
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

```
SELECT
  name,
  level,
  CAST(hp as numeric)/mp AS ratio
FROM
  character
```

For each character, show its name and try to divide its mp by its wisdom. Filter out rows with 0 wisdom points. Name the second column ratio.

```
SELECT
  name,
  CAST(mp as numeric)/wisdom AS ratio
FROM
  character
WHERE wisdom != 0
```

For each character, show its name and calculate how many health points will be left if the player decides to sacrifice the maximum number of hp. Name the second column health_points.

```
SELECT
  name,
  mod(hp,7) AS health_points
FROM
  character
```

For each character, show its name, its account_balance and its account_balance rounded to the nearest integer (name this column rounded_account_balance). Notice how mathematical rounding is applied.

```
SELECT
  name,
  account_balance,
  round(account_balance) AS rounded_account_balance
FROM
  character
```

Show each character's name together with its original account_balance and account_balance rounded to a single decimal place (name this column rounded_account_balance).

```
SELECT
  name,
  account_balance,
  round(account_balance, 1) AS rounded_account_balance
FROM
  character
```

Let's check some rounding functions in the next few exercises. Show the character's name together with its weight, followed by the weight rounded up (name this column weight_rounded_up.

```
SELECT
  name,
  weight,
  ceil(weight) AS weight_rounded_up
FROM
  character
```

Show each character's name together with its account_balance followed by the account_balance rounded down. Name the last column floor.

```
SELECT
  name,
  account_balance,
  floor(account_balance) AS floor
FROM
  character
```

Show the character's name together with its stat_modifier, followed by the stat_modifier rounded with the function trunc(x,p) to a single decimal place. Name the last column trunc. Observe how it behaves for positive and negative numbers.

```
SELECT
  name,
  stat_modifier,
  trunc(stat_modifier,1) AS trunc
FROM
  character
```

For each character, show its stat_modifier and its absolute value (as absolute_value).

```
SELECT
  stat_modifier,
  abs(stat_modifier) AS absolute_value
FROM
  character
```

Show each character's name, strength and calculate the square root of strength for each of them. Name the last column square_root.

```
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

```
SELECT
  name,
  round(weight/(height*height)) AS bmi
FROM
  character
```
A healing potion costs 50 gold coins. For each character with an account balance of at least 100, calculate how many healing potions it can buy (column name potion_amount) and how much money it will have left as a result of that purchase (column change). Round the first column down to integers.

```
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

```
SELECT
  name,
  round((strength + (hp/4.0)) * abs(stat_modifier),2) AS damage
FROM
  character
WHERE
  level >= 3 AND class = 'warrior'
```

For players with IDs equal to 1 and 5, show firstname and lastname together with the level, height rounded to two decimal places, and account_balance rounded down. The column names should be: firstname, lastname, level, height, and account_balance.

```
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

