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

