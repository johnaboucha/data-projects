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

