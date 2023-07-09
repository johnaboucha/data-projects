# Formulas

Select the **last item** in a column.

```
=OFFSET(A1, COUNTA(A:A)-1, 0)
```

If a value is **found** or **not found** in a string, return one value or another.

```
=IF(ISNUMBER(SEARCH(find_text, within_text)), value_if_true, value_if_false)
```