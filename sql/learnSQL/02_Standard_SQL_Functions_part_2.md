# Standard SQL Functions Part 2

## Nulls

_product_

| column name  | example value |
| ----| ---- |
| id   | 1 |
| category | bedroom |
| name	 | Vaatekaappi |
| type | wardrobe |
|	price	| 299.99 |
| launch_date | 2015-08-01 |
| description | lorem ipsum |
| production_area | UE |
| shipping_cost | 10.00 |

_advertisement_

| id  | product_id | country | start_date | end_date |
| ----| ---- | ---- | ---- | ---- |
| 1  | 1 | Poland | 2016-04-10 | null |

Select name for all products whose launch_date is not a null.

```sql
SELECT name
FROM product
WHERE launch_date IS NOT NULL
```

Now, select name for all the products whose category is NULL.

```sql
SELECT name
FROM product
WHERE category IS NULL
```

Select the name for all the products with a price different than 299.99. Include NULLs!

```sql
SELECT name
FROM product
WHERE price != 299.99
  OR price IS NULL
```

Select the name for all products together with their categories and types. Exclude rows with category 'kitchen' and those rows which have no category set.

```sql
SELECT
  name,
  category,
  type
FROM product
WHERE category != 'kitchen'
```

Mr Amund released a certain series of products on April 30, 2014. Now, he would like to get all the names, categories and types of those products which were introduced any time later or which will be introduced in the future (i.e., they don't have a launch_date yet). Write the proper query.

```sql
SELECT
  name,
  category,
  type
FROM product
WHERE launch_date > '2014-04-30'
  OR launch_date IS NULL
```

Select all the columns for products from categories: kitchen, bathroom, or with unknown category.

```sql
SELECT *
FROM product
WHERE category IN ('kitchen', 'bathroom')
  OR category IS NULL
```

A new tax has been imposed on all the products, which is 2% of the final price. Show all product names with their old price (column name old_price) and new price rounded to two decimal points (column name new_price). Note what happens when there is no price.

```sql
SELECT
  name,
  price as old_price,
  round(price * 1.02,2) as new_price
FROM
  product
```

Show the names of all products shorter than 8 characters or those which have a NULL name. Show the description as the second column so that Mr Amund can think of a (short) name if it's missing for a certain product before the advertisement is published.

```sql
SELECT
  name,
  description
FROM product
WHERE length(name) < 8
  OR name IS NULL
```

Select the names and categories for all products. If any of the columns is a NULL, write 'n/a' instead. Name the columns name and category.

```sql
SELECT
  COALESCE(name, 'n/a') as name,
  COALESCE(category, 'n/a') as category
FROM product
```

Show the following sentence: Product X is made in Y. Where X is the name and Y is the production_area. If the name is not provided, write 'unknown name' instead. If the production_area is not provided, write 'unknown area'. Name the column sentence.

```sql
SELECT
  'Product ' || COALESCE(name, 'unknown name') ||
  ' is made in ' || COALESCE(production_area, 'unknown area') || '.' as sentence
FROM product
```

The company organized a discount campaign on all prices: -10%.

Show the names of all products together with their price and the new prices (name the column new_price). If there is no price, show 0.00 in the column new_price. Round the result to two decimal points.

```sql
SELECT
  name,
  price,
  round(COALESCE(price, 0) * .9,2) as new_price
FROM product
```

Show the name of each product together with its type. When the type is unknown, show the category. When the category is missing too, show 'No clue what that is'. Name the column type.

```sql
SELECT
  name,
  COALESCE(type, category, 'No clue what that is') as type
FROM product
```

Today, the customers at Highland Furniture get a special offer: the price of each product is decreased by 1.99! This means that some products may even be free!

Our customer has $1,000.00 and would like to know how many products of each kind they could buy.

Show the name of each product and a second column named quantity: Divide the sum of 1000.00 by the new price of each product. Watch out for division by 0 and return a NULL instead.

```sql
SELECT
  name,
  1000.00 / NULLIF(price - 1.99,0) as quantity
FROM product
```

Show the name of each product and the column ratio calculated in the following way: the price of each product in relation to its shipping_cost in percent, rounded to an integer value (e.g. 31).

If the shipping_cost is 0.00, show NULL instead.

```sql
SELECT
  name,
  round(price / NULLIF (shipping_cost, 0.00) * 100) as ratio
FROM product
```

The promotion is as follows: every customer who orders a product and picks it up on their own (i.e., no shipping required) can buy it at a special price: the initial price MINUS the shipping_cost! If the shipping_cost is greater than the price itself, then the customer still pays the difference (i.e. the absolute value of price - shipping_cost).

Our customer has $1,000.00 again and wants to know how many products of each kind they could buy. Show each product name with the column quantity which calculates the number of products. Drop the decimal part. If you get a price of 0.00, show NULL instead.

```sql
SELECT
  name,
  floor(1000.00 / NULLIF(abs(price - shipping_cost),0.00)) as quantity
FROM product
```

Select the name and launch_date of all products for which the launch date is not February 25, 2015.

```sql
SELECT
  name,
  launch_date
FROM product
WHERE launch_date != '2015-02-25'
  OR launch_date IS NULL
```

Show the name of all products together with their price. If the price is equal to 19.99 change it to NULL. However, make sure there aren't any NULLs in the price column, show 'unknown' instead of them. Make sure the latter column is named price.

```sql
SELECT
  name,
  COALESCE(CAST(NULLIF(price, 19.99) as varchar), 'unknown', cast(price as varchar)) as price
FROM product
```

Show the id, name and category of all products in the descending order of names.

```sql
SELECT
  id,
  name,
  category
FROM product
ORDER BY name DESC
```

For each advertisement, show its id, country, start_date and end_date together with the name of the product advertised.

If there is no start_date or no end_date, show 'n/a' instead. If there is no name of the product, show 'no name'. Order the results by the start_date, with the oldest dates shown first. The column names should be: id, country, start_date, end_date, and name.

```sql
SELECT
  advertisement.id,
  country,
  COALESCE(CAST(start_date as varchar), 'n/a') as start_date,
  COALESCE(CAST(end_date as varchar), 'n/a') as end_date,
  COALESCE(name, 'no name') as name
FROM
  advertisement
JOIN product
  ON product_id = product.id
ORDER BY start_date
```

For each product with a non-NULL name, show its id and the number of advertisements associated with it. Sort the results by the id in descending order.

```sql
SELECT
  product.id,
  COUNT(product_id)
FROM product
LEFT JOIN advertisement
  ON product.id = product_id
WHERE name IS NOT NULL
GROUP BY product.id
ORDER BY product.id DESC
```

or each advertisement, show its id and the following sentence (as sentence):

The advertisement with id {id} for the product {name} was started on {date}.
Replace {id}, {name} and {date} with the proper columns. If there is no name, show the product id, if the product id is missing too, show 'no name'. If there is no started_date, show 'n/a'. Ignore products with missing price.

```sql
SELECT
  ad.id,
  'The advertisement with id ' || ad.id ||
  ' for the product ' || COALESCE(name, CAST(product.id as varchar), 'no name') || ' was started on ' ||
  COALESCE(CAST(start_date as varchar), 'n/a') || '.' as sentence
FROM advertisement as ad
JOIN product
  ON product_id = product.id
WHERE price IS NOT NULL
```

## Aggregate Functions

_client_

| client_id  | name |
| ----| ---- |
| 1   | Western Technology |
| … | … |

_translater_

| translator_id  | first_name | last_name | start_date |
| ----| ---- | ---- | ---- |
| 1   | Alicia | Potts | 2015-05-31 |
| … | … | … | … |

_project_

| column  | example value |
| ----| ---- |
| project_id   | 1 |
| client_id | 1 |
| translator_id | 1 |
| start | 2023-03-03 14:30:00+00 |
| deadline | 2023-03-04 10:00:00+00 |
| price | 40 |
| words | 250 |
| lang_from | PL |
| lang_to | ES |
| feedback | null |

Count the number of all the projects for the client with id 1. Name the column projects_no.

```sql
SELECT count(*) as projects_no
FROM project
WHERE client_id = 1
```

Count all the rows in the table project (name the column all_projects), then count those rows where price is not NULL (name the column projects_with_price).

```sql
SELECT
  count(*) as all_projects,
  count(price) as projects_with_price
FROM project
```

The agency manager gets a bonus of 5 for each project which got some feedback.

Show the theoretical sum the project manager could get if all the projects got some feedback (theoretical_bonus) and the actual bonus (actual_bonus).

```sql
SELECT
  COUNT(*) * 5 as theoretical_bonus,
  COUNT(feedback) * 5 as actual_bonus
FROM project
```

The feedback of 0 doesn't mean anything to agency managers. They want to find out how many projects got meaningful (non-zero) feedback.

Count the number of project with non-zero feedback. Name the column projects_no.

```sql
SELECT
  COUNT(feedback) as projects_no
FROM project
WHERE feedback != 0
```

Count the number of translators that did translate at least one project. Name the column translator_no

```sql
SELECT
  COUNT(DISTINCT translator_id) as translator_no
FROM project
```

Count how many distinct rates per word our translation agency uses. To obtain the rate for one word, divide the price by number of words. Name the column rates.

Make sure not to use integer division.

```sql
SELECT
  COUNT(DISTINCT CAST(price as FLOAT) / words) as rates
FROM project
```

Show clients IDs (client_id) together with the number of projects they have commissioned. Name the second column projects_no. Don't show clients without any project.

```sql
SELECT
  client_id,
  COUNT(project_id) as projects_no
FROM project
GROUP BY client_id
```

For each project, show the client_id and the number of projects commissioned by that specific client which have some feedback (zero if there are no such projects). Name the second column projects_no.

```sql
SELECT
  client_id,
  COUNT(feedback) as projects_no
FROM project
GROUP BY client_id
```

For each translator in table project, show their ID (as translator_id) and the number of clients they have worked for. Name the second column clients_no.

```sql
SELECT
  translator_id,
  COUNT(DISTINCT client_id) as clients_no
FROM project
GROUP BY translator_id
```

The rate per word is the price divided by the number of words. For every translator, show ID together with number of distinct rates per word they used in their projects. Name the second column rates_no.

```sql
SELECT
  translator_id,
  COUNT(DISTINCT CAST(price as FLOAT) / words) as rates_no
FROM project
GROUP BY translator_id
```

Show the first and last name of each translator together with the number of projects they have completed (0 if there are no such projects). Name the last column projects_no.

```sql
SELECT
  first_name,
  last_name,
  COUNT(project_id) as projects_no
FROM translator
LEFT JOIN project
  ON project.translator_id = translator.translator_id
GROUP BY last_name, first_name
```

For each client, show their name, the number of projects they have commissioned and the number of translators that have worked for that client. Do not count projects with a deadline within the last 3 months. The column names should be: name, projects_no, and translators_no.

```sql
SELECT
  name,
  COUNT(project_id) as projects_no,
  COUNT(DISTINCT translator_id) as translators_no
FROM
  client
LEFT JOIN project
  ON client.client_id = project.client_id
WHERE deadline < CURRENT_TIMESTAMP - INTERVAL '3' MONTH
GROUP BY name
```

For each language combination (e.g. EN to ES, ES to EN, DE to PL etc.) show lang_from, lang_to and two more columns:

1. the number of projects completed in this combination (as projects_no).
1. the number of translators that have completed at least one project in that language combination (as translators_no).

All completed projects are present in the project table.

```sql
SELECT
  lang_from,
  lang_to,
  COUNT(project_id) as projects_no,
  COUNT(DISTINCT translator_id) as translators_no
FROM project
GROUP BY lang_from, lang_to
```

Find the average number of words from all projects with Spanish (ES) as the target language. Name the column average.

```sql
SELECT AVG(words) as average
FROM project
WHERE lang_to = 'ES'
```

Show each translator's ID together with the average number of words they had to translate in all their projects (as average). Exclude project with ID 6 – it was cancelled.

```sql
SELECT
  translator_id,
  AVG(words) as average
FROM project
WHERE project_id != 6
GROUP BY translator_id
```

Take a look at the three projects of Adriana Fuentes, translator with ID 5. Two of them have a NULL number of words.

Find the average number of words for all the projects of this translator. Name the column average. What do you think the average will be?

```sql
SELECT
  AVG(words) as average
FROM project
WHERE translator_id = 5
```

Let's calculate the average number of words in all the projects done by translator with id = 5 again.

This time, though, exclude project_id = 13 from the rows so that the only two projects left are the ones with a NULL value. Name column average.

```sql
SELECT
  AVG(words) as average
FROM project
WHERE translator_id = 5 AND project_id != 13
```

Find the average number of words per project translated so far by Adriana Fuentes (translator_id = 5). Assume that NULL in column words means that nothing has been translated yet (treat it as words = 0). Name the result average.

```sql
SELECT
  AVG(COALESCE(words, 0)) as average
FROM project
WHERE translator_id = 5
```

For client with ID 10, find the average price (as avg_price) and average distinct price (as avg_distinct_price).

```sql
SELECT
  AVG(price) as avg_price,
  AVG(DISTINCT price) as avg_distinct_price
FROM project
WHERE client_id = 10
```

For each client in the project table, show the client ID and find the average price for all their projects rounded to integer values. Column names should be client_id and rounded_average.

```sql
SELECT
  client_id,
  ROUND(AVG(price)) as rounded_average
FROM project
GROUP BY client_id
```

For each translator, show their first and last name together with the average price of all the projects they have completed (as rounded_average). Round the price to two decimal places. Count only the projects with the number of words greater than 500. Exclude translators who have completed fewer than 2 projects (all completed projects are present in the project table).

```sql
SELECT
  first_name,
  last_name,
  ROUND(AVG(price), 2) as rounded_average
FROM translator
LEFT JOIN project
  ON translator.translator_id = project.translator_id
WHERE words > 500
GROUP BY first_name, last_name
HAVING COUNT(*) > 1
```

Find the total number of words translated in all the projects. Name the column sum.

```sql
SELECT
  SUM(words)
FROM project
```

Remember the translator with translator_id = 5 Adriana Fuentes? Let's go back there again. Sum all the words from all her projects except for project_id = 13 so that you only sum NULLs. Name the column sum.

```sql
SELECT SUM(words)
FROM project
WHERE translator_id = 5 AND project_id != 13
```

Show translator ids with the sum of the prices of all the projects they have completed (as sum). Exclude project_id = 13 so that one of the translators has NULL prices only. Instead of that NULL sum, show 0.

```sql
SELECT
  translator_id,
  COALESCE(SUM(price),0) as sum
FROM project
WHERE project_id != 13
GROUP BY translator_id
```

For client with id = 10, find the sum of the number of words in their projects, and the sum of the distinct number of words in their project. Name the columns sum and distinct_sum. Note that the results differ.

```sql
SELECT
  SUM(words) as sum,
  SUM(DISTINCT words) as distinct_sum
FROM project
WHERE client_id = 10
```

For each translator, show their first and last name, followed by:

- the sum of the prices for all the projects they have completed (as sum).
- the maximum price (as max).
- the minimum price (as min).

Make sure that all translators are shown, and if any of these values turns out to be NULL, show 0 instead.

```sql
SELECT
  first_name,
  last_name,
  COALESCE(SUM(price), 0) as sum,
  COALESCE(MAX(price), 0) as max,
  COALESCE(MIN(price), 0) as min
FROM translator
LEFT JOIN project
  ON translator.translator_id = project.translator_id
GROUP BY first_name, last_name
```

Count the number of all projects, the number of clients that have commissioned at least one project (that is, count them once even if they commissioned more than one project) and the average price for all the projects. Name the columns: projects_no, clients_no, and average_price.

```sql
SELECT
  COUNT(*) as projects_no,
  COUNT(DISTINCT client_id) as clients_no,
  AVG(price) as average_price
FROM project
```

For each translator, show:

- their first and last name.
- the number of projects they have completed (as projects_no).
- the highest number of words among their projects (as max_words).
- the average price among their projects rounded to integer values (as rounded_avg_price).

```sql
SELECT
  first_name,
  last_name,
  COUNT(project_id) as projects_no,
  MAX(words) as max_words,
  ROUND(AVG(price)) as rounded_avg_price
FROM translator
LEFT JOIN project
  ON translator.translator_id = project.translator_id
GROUP BY last_name, first_name
```

For each language combination in table project (e.g. EN to ES, ES to EN, DE to PL etc.) show lang_from, lang_to and the average price in this combination (as avg_price). When calculating the average, treat NULL values as 0.

```sql
SELECT
  lang_from,
  lang_to,
  AVG(COALESCE(price, 0)) as avg_price
FROM project
GROUP BY lang_from, lang_to
```

Show each client's name together with the number of projects they have completed (name this column projects_no). Show only those clients who have commissioned more than 1 project.

```sql
SELECT
  name,
  COUNT(project_id) as projects_no
FROM client
JOIN project
  ON client.client_id = project.client_id
GROUP BY name
HAVING COUNT(*) > 1
```

## CASE WHEN

_candidate_

| id  | first_name | last_name | score_math | score_language | preferred_contact |
| ----| ---- | ---- | ---- | ---- | ---- |
| 1 | Troy | Gray | 43 | 64 | mobile |
| … | … | … | … | … | … |

_course_

| id  | name | place_limit | scholarship | graduate_satisfaction |
| ----| ---- | ---- | ---- | ---- |
| 1 | Viking Studies | 10 | f | 67 |
| … | … | … | … | … |

_application_

| candidate_id  | course_id | fee | pay_date | status |
| ----| ---- | ---- | ---- | ---- |
| 5 | 1 | 50 | 2015-06-01 | accepted |
| … | … | … | … | … |

For each degree course, show its name and a second column based on the column place_limit:

- If it's 5, write 'few places'.
- if it's 10, write 'average number of places'.
- If it's 15 – 'numerous places'.
- If the result is something else, show 'other'.

```sql
SELECT
  name,
  CASE place_limit
    WHEN 5 THEN 'few places'
    WHEN 10 THEN 'average number of places'
    WHEN 15 THEN 'numerous places'
    ELSE 'other'
  END
FROM course
```

For each application, show the candidate ID (as candidate_id) and the course_id along with the following information: if the status is accepted, show 1. Otherwise, show 0. Name the last column accepted.

```sql
SELECT
  candidate_id,
  course_id,
  CASE status
    WHEN 'accepted' THEN 1
    ELSE 0
  END as accepted
FROM application
```

For table candidate, show each candidate's id, preferred_contact and the third column which will print:

- 'traditional' if the preferred_contact is 'mail'.
- 'modern' if it's 'mobile' or 'e-mail'.
- 'other' otherwise.

```sql
SELECT
  id,
  preferred_contact,
  CASE preferred_contact
    WHEN 'mail' THEN 'traditional'
    WHEN 'mobile' THEN 'modern'
    WHEN 'e-mail' THEN 'modern'
    ELSE 'other'
  END
FROM candidate
```

Show the following columns: name of the course, first and last name of the candidate (first_name, last_name), and a column fee_information which shows:

- 'high' when the fee is 50.
- 'low' in any other case.

Sort the rows by the name of the course in DESCending order.

```sql
SELECT
  name,
  first_name,
  last_name,
  CASE fee
    WHEN 50 THEN 'high'
    ELSE 'low'
  END as fee_information
FROM application
JOIN course
  ON application.course_id = course.id
JOIN candidate
  ON application.candidate_id = candidate.id
ORDER BY name DESC
```

Let's make a similar query for language skills. For each candidate, show their first and last name (first_name, last_name) along with a third column language_skills with the following possibilities:

- 'amazing linguist' (>80 language points).
- 'can speak a bit' (>50 language points).
- 'cannot speak a word' otherwise.

```sql
SELECT
  first_name,
  last_name,
  CASE
    WHEN score_language > 80 THEN 'amazing linguist'
    WHEN score_language > 50 THEN 'can speak a bit'
    ELSE 'cannot speak a word'
  END as language_skills
FROM candidate
```

For each candidate, show the first_name and last_name and a third column called overall_result: if the sum of score_math and score_language is 150 or more, show 'excellent'. If it's between 100 and 149, show 'good'. Otherwise, show 'poor'.

```sql
SELECT
  first_name,
  last_name,
  CASE
    WHEN score_math + score_language > 150 THEN 'excellent'
    WHEN score_math + score_language > 100 THEN 'good'
    ELSE 'poor'
  END as overall_result
FROM candidate
```

For each candidate, show the first_name and last_name along with the third column contact_info with the values:

- 'provided' when the column preferred_contact is not NULL.
- 'not provided' otherwise.

```sql
SELECT
  first_name,
  last_name,
  CASE
    WHEN preferred_contact IS NOT NULL THEN 'provided'
    ELSE 'not provided'
  END as contact_info
FROM candidate
```

Show each candidate's ID, score in math and a third column which will show:

- 'above average' for candidates with math score higher than 60.
- 'below average' for candidates with math score equal to or less than 60.
- For other results, show 'not available'.

The column names should be: id, score_math, and result. Note what happens to the rows with a NULL score.

```sql
SELECT
  id,
  score_math,
  CASE
    WHEN score_math > 60 THEN 'above average'
    WHEN score_math <= 60 THEN 'below average'
    ELSE 'not available'
  END as result
FROM candidate
```

The courses have different requirements, so let's check who can actually become a student of Ethical Hacking (id = 3). Show the name of this course, the first_name and last_name of the candidate who applied for this course, and another column called qualification. The rules are as follows:

- If the student got above 80 in math, show 'qualified'.
- If the score in math is above 70, but the score in language is above 50, show 'qualified' as well.
- If the score in math is between 50 and 70, show 'possible'.
- Otherwise, show 'rejected'.

```sql
SELECT
  name,
  first_name,
  last_name,
  CASE
    WHEN score_math > 80 THEN 'qualified'
    WHEN score_math > 70 AND score_language > 50 THEN 'qualified'
    WHEN score_math BETWEEN 50 AND 70 THEN 'possible'
    ELSE 'rejected'
  END as qualification
FROM application
JOIN candidate
  ON candidate.id = application.candidate_id
JOIN course
  ON application.course_id = course.id
WHERE course_id = 3
```

Calculate the total sum from fees paid on June 3, 2015 (column june_3rd) and the total sum from fees paid on June 4, 2015 (column june_4th).

```sql
SELECT
  SUM(CASE
      WHEN pay_date = '2015-06-03' THEN fee
      ELSE 0
     END) as june_3rd,
  SUM(CASE
      WHEN pay_date = '2015-06-04' THEN fee
      ELSE 0
    END) as june_4th
FROM application
```

Count the number of applications that have:

- a full fee of 50 (full_fee_sum).
- a fee of 10 (reduced_fee_sum).
- a fee of 0 (free_sum).
- a fee of NULL (null_fee_sum).

```sql
SELECT
  SUM(CASE WHEN fee = 50 THEN 1 ELSE 0 END) as full_fee_sum,
  SUM(CASE WHEN fee = 10 THEN 1 ELSE 0 END) as reduced_fee_sum,
  SUM(CASE WHEN fee = 0 THEN 1 ELSE 0 END) as free_sum,
  SUM(CASE WHEN fee IS NULL THEN 1 ELSE 0 END) as null_fee_sum
FROM application
```

Count the number of candidates with preferred_contact set to:

- 'mobile' (mobile_sum).
- 'e-mail' (email_sum).
- 'mail' (mail_sum).
- with NULL (null_sum).

Only count candidates whose score in math and score in language are both greater than or equal to 30.

```sql
SELECT
  SUM(CASE WHEN preferred_contact = 'mobile' THEN 1 ELSE 0 END) as mobile_sum,
  SUM(CASE WHEN preferred_contact = 'e-mail' THEN 1 ELSE 0 END) as email_sum,
  SUM(CASE WHEN preferred_contact = 'mail' THEN 1 ELSE 0 END) as mail_sum,
  SUM(CASE WHEN preferred_contact IS NULL THEN 1 ELSE 0 END) as null_sum
FROM candidate
WHERE score_math >= 30 AND score_language >= 30
```

Count the number of courses with possible scholarship (scholarship_present) and without them (scholarship_missing).

```sql
SELECT
  COUNT(CASE
        WHEN scholarship = 't' THEN scholarship END) as scholarship_present,
  COUNT(CASE
        WHEN scholarship = 'f' THEN scholarship END) as scholarship_missing
FROM course
```

Count the number of candidates who scored:

- at least 80 in math (good_math).
- at least 60 and less than 80 (average_math).
- or less than 60 (poor_math).

Show only those candidates who have provided a preferred_contact form.

```sql
SELECT
  COUNT(CASE WHEN score_math >= 80 THEN 1 END) as good_math,
  COUNT(CASE WHEN score_math BETWEEN 60 AND 80 THEN 1 END) as average_math,
  COUNT(CASE WHEN score_math < 60 THEN 1 END) as poor_math
FROM candidate
WHERE preferred_contact IS NOT NULL
```

Show how many students paid the full fee of 50 (full_fee_sum) and the reduced fee of 10 (reduced_fee_sum), but if a certain student paid the same amount for more than one degree course, count them only once.

```sql
SELECT
  COUNT(DISTINCT CASE
        WHEN fee = 50 THEN candidate_id END) as full_fee_sum,
  COUNT(DISTINCT CASE
        WHEN fee = 10 THEN candidate_id END) as reduced_fee_sum
FROM application
```

For each course, show its id (name the column course_id) and three more columns: accepted, pending and rejected, each containing the number of accepted, pending and rejected applications for that course, respectively. (Don't show the courses that aren't present in the application table.)

Sort the results by ID in ASCending order.

```sql
SELECT
  course_id,
  COUNT(CASE WHEN status = 'accepted' THEN 1 END) as accepted,
  COUNT(CASE WHEN status = 'pending' THEN 1 END) as pending,
  COUNT(CASE WHEN status = 'rejected' THEN 1 END) as rejected
FROM application
GROUP BY course_id
ORDER BY course_id
```

For each candidate, with at least one application, show their id and three more columns called count_accepted, count_rejected, count_pending. Each of these columns should count the number of times the candidate has been accepted to courses, rejected or where the decision is pending.

Sort the rows in ASCending order by the id.

```sql
SELECT
  candidate_id as id,
  COUNT(CASE WHEN status = 'accepted' THEN 1 END) as count_accepted,
  COUNT(CASE WHEN status = 'rejected' THEN 1 END) as count_rejected,
  COUNT(CASE WHEN status = 'pending' THEN 1 END) as count_pending
FROM application
GROUP BY id
ORDER BY id
```

For each degree course, show its ID (as id) and the number of distinct candidates who applied for this course and have the preferred_contact set to:

- 'mobile' (column: count_mobile),
- 'mail' (column: count_mail).

```sql
SELECT
  course_id as id,
  COUNT(DISTINCT CASE
        WHEN preferred_contact = 'mobile' THEN candidate_id END) as count_mobile,
  COUNT(DISTINCT CASE
        WHEN preferred_contact = 'mail' THEN candidate_id END) as count_mail
FROM application
JOIN candidate ON
  application.candidate_id = candidate.id
GROUP BY course_id
```

Show each preferred_contact form with the number of candidates who provided it (as candidates_count). Along with that, show a column entitled 'rating' and show:

- 'high' if there are more than 5 candidates with this form of contact.
- 'low' otherwise.

```sql
SELECT
  preferred_contact,
  COUNT(id) as candidates_count,
  CASE
    WHEN COUNT(id) > 5 THEN 'high'
    ELSE 'low'
  END as rating
FROM candidate
GROUP BY preferred_contact
```

Use the construction we have just shown you to count the number of candidates who scored:

- at least 70 in the language test ('good score').
- at least 40 ('average score').
- and below 40 ('poor score').

The column names should be case and count.

```sql
SELECT
  CASE
    WHEN score_language >= 70 THEN 'good score'
    WHEN score_language >= 40 THEN 'average score'
    ELSE 'poor score'
  END as case,
  COUNT(id) as count
FROM candidate
GROUP BY CASE
  WHEN score_language >= 70 THEN 'good score'
  WHEN score_language >= 40 THEN 'average score'
  ELSE 'poor score'
END
```

For each course, show its name and a second column based on the column graduate_satisfaction:

- if it's above 80, show 'satisfied'.
- if it's above 50, show 'moderately satisfied'.
- if it's 50 or less, show 'not satisfied'.

Name the second column satisfaction_level.

```sql
SELECT
  name,
  CASE
    WHEN graduate_satisfaction > 80 THEN 'satisfied'
    WHEN graduate_satisfaction > 50 THEN 'moderately satisfied'
    WHEN graduate_satisfaction <= 50 THEN 'not satisfied'
  END as satisfaction_level
FROM course
```

Show the number of students who scored at least 60 in both math and language (as versatile_candidates) and the number of students who scored below 40 in both of these tests (as poor_candidates). Don't include students with NULL preferred_contact.

```sql
SELECT
  COUNT(CASE
        WHEN score_math >= 60 AND score_language >= 60
        THEN 1 END) as versatile_candidates,
  COUNT(CASE
        WHEN score_math < 40 AND score_language < 40
        THEN 1 END) as poor_candidates
FROM candidate
WHERE preferred_contact IS NOT NULL
```

Show each degree course name and a second column called popularity. If at least 5 distinct candidates applied, show 'high', otherwise show 'low'.

```sql
SELECT
  name,
  CASE WHEN COUNT(DISTINCT candidate_id) > 5 THEN 'high' ELSE 'low' END as popularity
FROM course
JOIN application
  ON course.id = application.course_id
GROUP BY name
```

For each degree course, show its name, the place_limit, the number of people who applied for that degree course (as candidates_no) and yet another column popularity: if more people applied than place_limit, show 'overcrowded'. Otherwise, show 'within limit'.

```sql
SELECT
  name,
  place_limit,
  COUNT(DISTINCT candidate_id) as candidates_no,
  CASE
    WHEN COUNT(DISTINCT candidate_id) > place_limit THEN 'overcrowded'
    ELSE 'within limit' END as popularity
FROM course
LEFT JOIN application
  ON course.id = application.course_id
GROUP BY name, place_limit
```