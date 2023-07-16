# Microsoft Power BI Desktop

Power BI is a data visualization tool that can connect to different data sets, transform and clean data, create data models, and build interactive charts or graphs. It’s used by business intelligence analysts as well as data analysts and data scientists.

## Connecting and Shaping Data

Power BI contains two distinct environments:

- The **front-end** contains the Data, Model, and Report views, where most of the modeling, analysis, and reporting happens
- The **back-end** contains the Power Query Editor, where the raw data is extracted, transformed, and loaded (ETL)

Power BI has a robust library of data connectors that let you connect data from virtually anywhere. Examples include:

- Flat files and folders (Excel, .csv, etc…)
- Databases
- Power Platform
- Azure
- Online services (GitHub, Google Analytics, etc…)
- And many more

The Power Query Editor opens when you import data. There are 3 tabs where most of your work will be performed:

- Home tab, includes general settings and common table transformation tools
- Transform tab, includes tools to modify existing columns (splitting/grouping, extracting text, transposing, and so on
- Add Column tab, has tools to create new columns based on conditional rules, text operations, calculations, etc.

In the Home tab of Power Query Editor, you have the option to Keep or Remove rows and columns. There is a slight difference between them. The Keep Rows option will only keep the rows/columns you select, regardless of whether new rows/columns are added.

When importing data, Power Query will scan the first 200 rows of each column and automatically assign a **data type** to the column.

Power BI supports several types of **storage** and **connection modes**.

- Import, imported into memory, data less than 1GB, source data that doesn’t change frequently, no restrictions	on Power Query, data modeling, and DAX functions
- DirectQuery, when dataset is too large for in-memory, changes frequently, when data can only be accessed from original source
- Composite Models, a hybrid model of the previous two, combine DirectQuery with additional imported data
- Live Connection, create one dataset as a central source of truth, maybe Power BI service and Azure Analysis Service, all team members create reports from same source

Power Query has a native **Web Connector** that can scrape websites and looks for structured table data.

**Profiling tools** like column quality, column distribution, and column profile allow you to explore quality, composition, and distribution of data before loading it into the front-end.

- Column quality, shows percentage of values within a column that are valid, contain errors, or are empty
- Column distribution, provides sample distribution of data in a column
- Column profile provides an holistic view of data in a column, sample distribution, profiling statistics

Power Query Editor contains tools for working with Dates. Select a date column, then find Date & Time tools under Transform and Add Column. Useful for generating day of week, year, month name, and so on.

Sometimes dates formats need to be changed to correspond with the region of origin. Different parts of the world format their dates differently. Left-click the data type icon, select **Using Locale**, select Date type and locale.

To Generate a list of dates from the beginning of the year until now, use the following M-code:

```
= List.Dates(
    Source,
    Number.From(DateTime.LocalNow()) - Number.From(Source),
    #duration(1, 0, 0, 0)
)
```

**Index Columns** contain a list of sequential values that act as unique IDs for the rows in your table. Find under Add Column > Index Column.

**Conditional Columns** are like calculated fields in Tableau. They allow you to create new column data based on logical rules and conditions. Find under Add Column > Index Column.

When creating Calculated Columns it’s a best practice to transform data as close to the original source as possible, like Excel, SQL Server, etc… instead of relying on Power BI, to optimize performance and speed.

**Grouping** and aggregating data can be done under the Transform > Group by menu option. You can group by single or multiple columns and select from a variety of different aggregation methods.

**Pivoting** is the process of turning distinct row values into columns. Unpivoting is the reverse, turning distinct columns into rows.

**Merge Queries** is a drop down box in the Combine section of the Home tab of Power Query Editor. You can select two tables based on a common column, like JOIN in SQL. Though, usually better to define relationship within the data model.

**Append Queries** appears next to merge queries. This allows you to concatenate rows from two tables into a single one, like UNION in SQL.

Like other Microsoft apps, the file references, such as to .CSV files, are a full path name, not a relative file path. If you move your project folder, you will need to update file paths by going into the Home tab > Data source settings > Change Source.
