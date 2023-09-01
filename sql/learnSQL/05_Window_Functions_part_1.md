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

