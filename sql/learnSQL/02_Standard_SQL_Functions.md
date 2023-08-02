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