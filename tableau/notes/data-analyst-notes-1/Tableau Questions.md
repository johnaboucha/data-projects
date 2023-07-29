# Tableau Questions

## Question Set 01

1. Median functions return a value of in an ordered set of values, below and above which there are an equal number of values.

True

2. If you don't have all the data values you need on the visualization, it's a perfect time to use quick table calculations.

False

3. If your file type is not listed under Connect, the only option is to convert the file to supported format.

False

4. You can create forecasts with only a dimension and a measure.

False (you need a DATE dimension)

5. The Forecast Options dialog box includes:

Additive, Multiplicative, None

6. Parameters can be used as variables (inputs) for clustering

False

7. Measure filters are executed before dimension filters

False

8. Web pages can be included in the Tableau dashboard

True

9. What are custom fields that define a subset of data based on some conditions?

Sets


## Question Set 02

10. The COUNTD function returns the number of distinct items, including NULL.

False

11. In an aggregated calculation, you cannot combine an aggregated value and a non-aggregated value.

True

12. When are linear trend lines considered significant?

When p-value <= 0.05

13. What are two methods Tableau uses to determine seasonality in forecasting?

Temporal and non-temporal

14. What Tableau option will distinguish marks (single data points) and call out their position on the X/Y axis in the view?

Drop Lines

15. You can manually sort data using the Legend.

True

16. Level Of Detail (LOD) filters are applied before context filters.

False

17. Bins can be used for continuous measure

True

18. In dashboards, strings and dates slow the performance more than numbers and booleans.

True

## Question Set 03

19. Measures are aggregated by default. The type of aggregation is:

Depends on the context of the view

20. How do you calculate GDP per capita?

SUM([GDP]) / SUM(POPULATION)

21. Relationship are represented by _____ and operate at the _____

Noodles, logical layer

22. Tableau displays measures over time as a:

Line chart

23. How many categories can a pie chart display effectively?

2 to 5

24. What is an example of a Date Part?

November

25. What are the benefits of using Data Extracts?

Use Data offline, Faster to work with, Improved performance

26. What shape do Heat Maps use by default?

Square

27. _____ refers to the level of detail for a piece of data, wherever you are looking.

Data granularity

28. Dimensions containing _____ and _____ values cannot be continuous.

Boolean, String

## Question Set 04

29. When you drop a continuous field on Color, Tableau displays a quantitative legend with a _____ range of colors.

Continuous (or gradient)

30. You just added "SUM(Profit)" to the columns shelf. This creates:

A horizontal axis

31. What represents a valid method to create a bullet graph with the least number of fields?

2 measures

32. We can join a maximum of _____ number of tables

32

33. What chart type makes use of 'binned' data?

Histogram

34. It is not possible to blend axes for multiple measures into a single axis.

False

35. What is stored in a .tds file?

Metadata edits, calculated fields, data connection information

36. What are valid objects when creating a dashboard in Tableau?

Web page, image, text, extension

37. _____ enables us to create workbooks and views, dashboards, and data sources in Tableau Desktop, and then publish content to our own server.

Tableau Server

## Question Set 05

38. Default path for supporting files, data sources, etc...

Documents > My Tableau Repository

39. All rows from both tables are returned on an INNER JOIN

False

40. When you first apply a filter and THEN show the Top N, which filter would you use?

Context filter

41. SUM is a table calculation

False

42. What is the main thing you do after clicking Sheet 1 from the data source page

Create visualizations to analyze data

43. Is it possible to use measures in the same view multiple times (SUM and AVG)?

True

44. What can you use to create a Histogram?

1 measure

45. What is a reason to use Stacked Bar Chart?

To visualize parts of the whole, to visualize complex information with fewer bars/marks

46. After cleaning data source, creating calculated fields and renaming columns, what is the file type is saved?

.tds

## Question Set 06

47. _____ contains the visualizations, info to build vizzes, and a copy of the data source.

Tableau Packaged Workbook .twbx

48. What are valid options to define the scope of a reference line?

Table, Pane, Cell

49. You can create _____ for members in a dimension so labels appear differently in the view.

Aliases

50. You can _____ your data to combine two or more tables by appending values (rows) from one table to another

Union

51. What is required to create a trend line?

2 measures on opposing axes, or a date and a measure on opposing axes

52. What is false about Joins?

Never merged into a single table, more dynamic way than relationships to combine data

53. What is the result of LEFT("Tableau", 3)?

Tab

54. What fields cannot be deleted in Tableau?

Measure Values, Measure Names

55. What does the connected, pyramid-like icon mean?

Hierarchy

56. The row and column shelves contain:

Pills

57. When working with Excel, text file data, JSON file, .pdf file data, you can use _____ to union files across folders, and worksheets across workbooks; search is scoped to the selected connection.

Wildcard search

58. Tableau attempts to create relationships based on existing key constraints and matching fields to define the relationship. If it can't, then relating tables is not possible.

False

59. _____ is hosted by Tableau to share our visualizations publicly.

Tableau Public

60. When relating tables, the fields that define the relationships must have the same data type.

True

## Question Set 07

61. What type of data does T|F represent?

True or False

62. What is possible from the Data Source tab?

View all hidden files
See the table a field belongs to
See the field name in the original data source

63. A sheet cannot be used within a story directly. Instead sheets should be used within a dashboard and the dashboard used within a story.

False

64. _____ is a snapshot of the data that Tableau stores locally. Good for large datasets of which we only need a few fields.

Tableau Data Extract (.tde) or hyper (.hyper)

65. For a relative date filter, the default anchor is _____

Today's Date

66. A reference line cannot be added from the Analytics Pane.

False

67. To concatenate fields, they must be of the same data type

True

68. A quick table calculation needs:

None of these: SQL, Python, Java

69. You can use the _____ in Tableau to automatically clean/organize your data.

Data Interpreter

70. To customize links based on the data in your dashboard, you can automatically enter field values as _____ in URLs.

Parameters

71. What is True for Measure Names?

When working with a text table, added another measure to the view results in measure names being added as a Filter.

Contains names of all measures in your data, collected into a single field with discrete values

When added to the view, appear as row or column headers

72. What does the box in a box plot represent?

The interquartile range

73. A LEFT JOIN or INNER JOIN creates a row each time the join criteria is satisfied, which can result in duplicate rows. So use relationships instead.

True

74. _____ is a technique in Tableau that identifies marks with similar characteristics

Clustering

75. How to calculate Profit Ratio in Tableau?

SUM([Profit]) / SUM([Sales])


## Question Set 08

76. We can disaggregate data, to see all the marks in the view at the most detailed level of granularity.

True

77. A field that shows average sales is most likely:

An aggregated measure

78. We can use _____ as a static tool to open and interact with packaged workbooks...

Tableau Reader

79. The calculation [Ship Date] - [Order Date] will return:

Number of days between these dates

80. What are options to export data used to build the view/visualization?

CSV, MS Access Database

81. Which of the following Actions are interactive elements that can be added to the dashboard for users?

URL action
Filter action
Highlight action

82. Given a map, which of the following fields can be placed on Size, Shape, Detail, Color in the same order.

Sales, State, Country, Profit

83. Is it possible to deploy a URL action on a dashboard object to open a web page?

Yes, with a Web-Page object

84. What is a good reason to use a bullet graph?

Comparing actual sales against target sales

85. How can you manually assign geographic roles to a dimension field from the data pane?

Right click the dimension > Geographic role > assign the appropriate role

86. Which of the following chart types always includes bars sorted in descending order?

Pareto Chart

87. It's possible to change the Geographic Role of a dimension

True

88. LEFT JOIN returns all the rows from the left table with matching rows in the right table.

True

89. Tableau can create worksheet-specific filters

True

90. When changing data sources, fields may be marked with a [!]. Now what?

Replace References

## Question Set 09

91. The icon associated with Groups is:

Paper Clip

92. Which of the following is not a Trend Line Model?

Binomial Trend Line

93. How do you identify a continuous field in Tableau

Green Pill

94. For creating variable sized bins we use:

Calculated Fields

95. What is True about Viz animations?

Tableau enables animations by default

Sequential animations take more time but complex changes are more clearly presented from step by step

It's possible to turn them off for the entire workbook at once

96. What does it imply if a field has a blue background?

Discrete

97. Enabling any other type of sort clears the manual sort we create

True

98. When using a Blend, what is the color of tick-mark on the primary and secondary sources.

Blue, Orange

99. A Tableau support case can be opened, how?

Using the support option on the Tableau website

100. Which of the following returns the Absolute Value of a given number?

ABS(Number)

101. Sets can be created on Measures

False

102. Context Filters are executed after Data Source Filters

True

103. What are valid ways to trigger actions for a Dashboard?

Select
Hover
Menu

104. Dates in Tableau are typically treated as:

Dimensions

## Question Set 10

105. When there are positive and negative values for a field, the default color range is known as a _____ palette.

Diverging

106. In a bar chart, when we group by labels, what happens?

A new mark (bar) is created, which consolidates all members of the group

107. A union of two tables usually results in an:

increase in the number of rows

108. A dimension on the Column shelf, a measure on the Rows shelf. What would create a stacked bar chart?

Dragging another dimension onto Color in the Marks card

109. How can you format an axis as Bold in Tableau?

Right-clicking on axis > Format..., then setting its font to Bold

110. There are 7 marks on screen from 1 dimension and 1 measure. Dragging another measure to view results in:

If dragged to column, 7 marks, creating a scatterplot segmented by the dimension
If dragged to row, 14 marks, two rows, the second containing the new measure

111. What helps us focus on specific data without removing data from visualization?

Highlighters

112. What type of set can we use to compare members?

Combined sets

113. What do the colors Blue and Green represent?

Discrete and Continuous

114. Which of the following DO NOT need a quick table calculation?

Standard Deviation

115. Which of the following are valid Dashboard size options?

Automatic
Fixed
Range

116. Which of the following lets you group related dashboard items together?

Layout Containers

117. Why direct connection to the extract is not recommended?

The extract cannot be refreshed

118. What is the path to change the data type for a field in the view?

Right click the field in the Data pane -> Change Data Type -> Choose the appropriate data type



