# SQL Basics

An intro course on the basics of SQL.

Car table schema:


| VIN | BRAND | MODEL | PRICE | PRODUCTION_YEAR |
| ---- | ----| ---- | ---- | ---- |
| LJCPCBLCX14500264 | FORD | Focus | 8000.00 | 2005 |
| … | … | … | … | … |

VIN	BRAND	MODEL	PRICE	PRODUCTION_YEAR
	Ford	Focus	8000.00	2005

Select all data from the car table.

```
SELECT * FROM CAR;
```