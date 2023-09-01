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