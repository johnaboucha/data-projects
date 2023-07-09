# Formulas and Functions

## Excel Formula Basics

Excel has almost 500 functions. They are not case-sensitive and not always required, such as =A1+B1. Most functions require arguments but some don’t, such as TODAY().

By default, Excel uses **relative** references. The cell references change as the formula is copied to new cells. Use the **$** symbol to fix columns and rows, such as $A1 or $B$2.

Six of the most **common error types**:

- ######, column isn’t wide enough to display values
- #NAME?, Excel doesn’t recognize text in formula
- #VALUE!, formula has the wrong type of value as a parameter
- #DIV/0!, dividing by zero
- #REF!, formula referencing cell that is no longer valid
- #N/A, formula cannot find referenced value, common with lookup functions that cannot find value in range

Under the Formulas tab, there are several **formula auditing** tools that are useful in debugging a chain of formulas and you’re not sure where the break is originating from. Typical tools are:

- Trace Precedents, identifies cells that affect the value of the one selected
- Trace Dependents, identify cells that are affected by the value of the one selected
- Show Formulas, displays formulas in worksheet as text
- Evaluate Formula, allows you to step through the formula’s calculations in the selected cell
- Error Checking, scans worksheet for any errors and provides a summary of options

Excel has many keyboard **shortcuts**. A few common shortcuts include:

- Ctrl + Arrow, jumps to last cell in data region
- Ctrl + Shift + Arrow, selects the range from current cell to the last cell in data region
- Ctrl + Home/End, jumps to the Home (top left) or End (bottom right) of region
- Ctrl + ., jumps to each corner of a selected data range
- Ctrl + PgUp/PgDn, switches worksheet tabs

Excel also has **function key shortcuts**. Many are contextual.

- F1
  - Launches the Excel help pane
  - Links to the MS support website
- F2
  - Allow you to edit the active cell
  - Highlights cells referenced by the active formula
- F4
  - Repeats the last action taken
  - Toggles absolute/relative cell references, cycles through (A1 > $A$1 > A$1 > $A1 > A1)
- F9
  - Calculates all workbook formulas
  - Evaluates each function argument inside formula bar

And then there are **Alt Key Tips**. These allow you to access tools from the Ribbon Menu and sub-menus with just the keyboard. It highlights each menu option with a letter and lets you drill down through the menus.

**Data Validation** restricts the values that a user can enter into a given cell. Found under the Data menu > Data Tools > Data validation button. Restrictions include:

- Number type, whole or decimal
- Value, between, less than, greater than, etc…
- List of items, within a cell range or manual list
- Date/Time, between, less than, greater than, etc…
- Text length, between, less than, greater than, etc…
- Custom, formula based

## Logical Operators

**IF statements** are good to know. The structure of the IF-statement is below:

=IF(logical_test, [Value if True], [Value if False])

An example would be:

=IF(A1>60, “Tall”, “Short”)

Nested IF statements allow you to test multiple conditions, such as =IF(condition, value if true, IF(another condition, true value, false value)).

Excel’s **AND** and **OR** statements allow you to test multiple logical tests at once. An example would be:

=IF(OR(A2=“Rain”,A2=“Snow”),”Wet”,”Dry”)

To evaluate if a statement is not True, use the **NOT** statement or  “<>” operator.

The **IFERROR** statement allows you to check if there’s an error in the formula or value and replace those with a value of your choosing.

=IFERROR(value, if error occurred value)

Lastly, Excel offers several different IS statements that check wither a certain condition is true:

- ISBLANK, checks if is cell blank
- ISNUMBER, checks if cell value is a number
- ISTEXT, checks if cell value is text
- ISERROR, checks if cell produces an error
- ISEVEN, checks if cell value is even
- ISODD, checks if cell value is odd
- ISLOGICAL, checks if cell value is a logical operator
- ISFORMULA, checks if cell value is a formula

## Statistical Functions

Excel has many different statistical functions built-in. Commonly used ones are:

- COUNT()
- AVERAGE()
- MEDIAN()
- MODE()
- MIN()
- MAX()
- VAR()
- STDEV()

There is also SMALL, LARGE, RANK, and PERCENTRANK. The Large and Small functions return the nth-largest or smallest value in a range of values.

=LARGE(A2:A10,2), returns second largest value

Rank works kind of the same. You pass in a value, followed by a range of values, or an array, and the function returns the rank.

=RANK(A2,A2:A10), might return 2 if it’s the second largest value

PercentRank works the same as rank but returns the percentage instead. The parameters are in a slightly different order:

=PERCENTRANK($A$2:$A$20, A2), maybe returns 18%

Random number generates exist in Excel as RAND() and RANDBETWEEN() functions. RAND() returns a number between 0 and 1 of up to 15 digits. RANDBETWEEN() returns an integer between two values you specify (inclusive).

=RANDBETWEEN(0,100)

The random functions will recalculate on any workbook change.

The SUMPRODUCT formula multiplies corresponding cells from multiple arrays and returns the sum of the products. The arrays must have the same dimensions.

=SUMPRODUCT(B2:B4,C2:c4), might return $5.50

This is just a quicker way of doing quantity*price in each row and summing the values. However, the formula is often used with filters to calculate products only for rows when they meet a certain criteria. For example, sum profits from apples:

=SUMPRODUCT((A2:A20=“Apple”)*C2:C20*D2:D20), may return $26.50

Conditional aggregation is possible with functions such as COUNTIF, SUMIF, and AVERAGEIF. Usage is similar to other Excel functions:

= SUMIF(A2:A20,”Apple”,B2:B20), maybe returns 200

Another set of aggregate functions exist if multiple conditions need to be true; COUNTIFS, SUMIFS, and AVERAGEIFS.

=COUNTIFS(B2:B30,”TX”,D2:D30,”>100”), maybe returns 7

## Lookup and Reference Functions

**Named arrays** are convenient when working with multiple lookup functions and formulas. You create these by selecting your data, then typing the name in the box left of the formula entry. Or, go to Formulas tab > Define Name or Name Manager to create the named array manually.

The **ROW([reference cell])** function returns the row number of the cell and the **ROWS(array)** function returns the number of rows of a given range of cells.

**COLUMN()** and **COLUMNS()** perform the same function as its rows counterpart.

Excel’s most common reference function is **VLOOKUP**. It’s good when you want to combine data from multiple arrays, like join statements in SQL. General usage is shown below.

```
=VLOOKUP(
  lookup_value, // the value you are trying to match in the table array
  table_array, // where you are looking up the value
  col_index_num, // the column number that has the data you’re looking for
  [range_lookup], // 0 for exact match, 1 for a similar match
)
```

VLOOKUP iterates row by row. If a data set is transposed and you need to iterate by column, use the HLOOKUP function.

If multiple values exist, Excel always returns the top or left most value when using the VLOOKUP function. Also, the lookup value must exist in the first column of the data. Otherwise, it doesn’t work!

**INDEX** is another related function that returns the value of a specific cell within an array. Typically used as part of more complex formulas, not usually by itself.

=INDEX(array, row_num, column_num)

**MATCH** is yet another related function. It returns the position of a specific value within a column or row. Likewise, it’s often used as part of more complex formulas.

=MATCH(lookup_value, lookup_array, [match_type])

INDEX and MATCH can be used together to act like a lookup function but can find values in any row or column in an array.

**XLOOKUP**, however, can retrieve values from a table or range by matching the lookup value. And it offers more flexibility than our previous lookup functions, returning arrays instead of single values.

=XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_model], [search_model])

Next, the **CHOOSE** function selects a value, cell reference, or function to perform from a list, based on a given index number.

=CHOOSE(index_num, value1, [value2], …)

List values passed into the CHOOSE function can be a mix of numbers, cell references, defines names, formulas, or text.

**OFFSET** is a function like INDEX but returns either the value of the cell within an array or a specific range of cells. Can be used to create dynamic arrays.

=OFFSET(reference, rows, columns, [height], [width])

## Text Functions

First, here are a set of simple text functions.

- TRIM, removes any leading or trailing spaces from text
- LOWER, converts the string to lowercase
- UPPER, converts the string to uppercase
- PROPER, converts the string to proper case
- CONCATENATE, combines values from multiple cells into one (can also use the **&** symbol)

Next, we can return substrings by using one of the following functions.

- LEFT, returns the number of characters from the left
- RIGHT, returns number of characters from right
- MID, returns specified number of characters from specified starting index
- LEN, returns length of string

Then there are two functions that can help when we need to treat cell values as something else.

- TEXT, converts a numeric value to text while using a specific format
- VALUE, converts a text string that represents a number into a numeric value

The **SEARCH** function returns the position number of the substring in a string. Returns a #VALUE! error if not found.

=SEARCH(find_text, within_text, [start_num])

The **FIND** function works the same as SEARCH but it’s case-sensitive.

Lastly, the **SUBSTITUTE** function lets you replace a substring with another string. Can be useful for when you want to create  single, unique identifier when trying to extract data from a string.

## Date and Time Functions

Every date in Excel has an associated date value, a number or decimal number representing the number of days since 1/1/1900.

Excel recognizes most types of dates but you can also use DATEVALUE and TIMEVALUE to convert unformatted dates too.

To format dates, right-click on the cell > Format Cells and select the Date type or create your own custom format.

When dragging a date cell, the auto-fill series will increment the values one day per cell, but you can change this to be weekdays, months, or years.

Two of the simpler date time functions are:

- TODAY, returns today’s date
- NOW, returns today’s date and time

Other day/time functions are self-explanatory:

- YEAR
- MONTH
- DAY
- HOUR
- MINUTE
- SECOND

More advanced date and time functions are listed below. These often require additional parameters.

- EOMONTH, calculates the last day of the month
- YEARFRAC, calculates the fraction of the year represented by the number of days between two dates
- WEEKDAY, returns the day of week as a number
- WORKDAY, returns a date that is a specified number of days before a given start date (confusing name, Thank Microsoft!)
- NETWORKDAYS, calculates the number of workdays between two dates
- DATEDIF, calculates the number of days between two dates

## Formula-Based Formatting

In Excel, it’s possible to create your own formula-based rules for customizing cells, columns, and rows.

Create custom rules under Home > Conditional Formatting > New Rule… > Use a formula to determine which cells to format

## Dynamic Array Formulas

A single formula can return many results and “spill” over to adjacent cells. The return arrays are called **dynamic arrays**. 

A simple example of dynamic arrays is summing two columns. In a new cell, create a formula and just select all the rows as the numerator and divide by all the other rows as the denominator. Excel creates a single formula in the first cell and spills the dynamic arrays downward.

The **spill range** contains the results of a single dynamic array formula. A **#SPILL!** error occurs when something is blocking the spill range.

Note that references used within dynamic array formulas **do not automatically resize** as you add new data. So either:

- Format the data as a table and used structured references (Ctrl + T to convert data into a table object)
- Select extra rows to accommodate any new records
- Define dynamic named ranges using OFFSET and COUNTA

Can also use the hashtag (C2#) to reference all values in a column. Useful for updating a dropdown box for data validation.

Dynamic array functions like SORT, FILTER, and UNIQUE unlock awesome capabilities in Excel.

- SORT, sorts an array by one or more columns contained within the array
- SORTBY, works like sort but can sort by columns in a different array
- FILTER, filters an array based on the specified criteria
- UNIQUE, removes duplicates from the array
- SEQUENCE, generates a one or two-dimensional array of sequential numbers
- RANDARRAY, generates a one or two-dimensional array of random numbers

The **FREQUENCY** function returns the frequency of values in a range based on specified intervals (bins). Good for when working with distributions.

The **TRANSPOSE** function flips a vertical range of cells to be horizontal, or vice versa.

The **CHOOSE** function can combine separate cell ranges into a single array that can be referenced by other formulas. Good for combining two spill ranges that need sorting.

The **LET** function allows you to create variables and use them in formulas. Helps you write cleaner formulas.

The **INDIRECT** function returns the reference specified by a text string and can be used to change a cell reference within a formula without changing the formula itself. Can be used in VLOOKUP functions to use an array within a cell rather than the cell itself.

The **HYPERLINK** function creates a link to a web location, another Excel document, or a location within a workbook.