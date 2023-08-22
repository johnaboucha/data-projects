# Insert, Update, and Delete data

_dish_

| id | type | name | price |
| ---- | ----| ---- | ---- |
| 1 | starter | name | 13 |
| … | … | … | … |

Add a dish called Cevapcici with an ID of 9 and a price of 27. It's a main course.

```sql
INSERT INTO dish 
VALUES (9, 'main course', 'Cevapcici', 27)
```

Add Kosovo Bread with ID of 10; it's a starter. We have yet to decide on the price, so omit it for now.

```sql
INSERT INTO dish (id, type, name)
VALUES (10, 'starter', 'Kosovo Bread')
```

Lots of Czech tourists are now coming to the restaurant, and they would like to try some of their national dishes. Let's give them:

A main course named Gulas s knedlikem (id 11, price 29).
A dessert named Vosi Hnizda (id 12, price 14).

```sql
INSERT INTO dish
VALUES
  (11, 'main course', 'Gulas s knedlikem', 29),
  (12, 'dessert', 'Vosi Hnizda', 14)
```

The owners of the restaurant complain that the item with ID 2 sells poorly. This might be because Spring Scrolls don't really sound like something particularly edible.

Correct the name by changing it to Spring Rolls.

```sql
UPDATE dish
SET name = 'Spring Rolls'
WHERE id = 2
```

Spring Rolls now sell like crazy, and nobody's interested in Prawn Salad (ID 1) anymore. We need to change its name to something more exotic—let's try Green Sea Dragon.

Apart from that, set the price at 10 to encourage customers to try the dish.

```sql
UPDATE dish
SET name = 'Green Sea Dragon', price = 10
WHERE id = 1
```

It's happy hour at our restaurant! Change the price of all main courses to 20.

```sql
UPDATE dish
SET price = 20
WHERE type = 'main course'
```

The restaurant owners think the starters are so delicious and popular that they should be much more expensive. Try multiplying their prices by 2.

```sql
UPDATE dish
SET price = price * 2
WHERE type = 'starter'
```

Oops, we've run out of sugar! Delete all desserts from our menu.

```sql
DELETE FROM dish
WHERE type = 'dessert'
```

## Advanced features of INSERT, UPDATE, and DELETE

_student_

| id | first_name | middle_name | last_name |
| ---- | ----| ---- | ---- |
| 1 | Mark | Johan | Adams |
| … | … | … | … |

_exam_

| column_name | sample value |
| ---- | ----|
| id | 1 |
| student_id | 1 |
| subject | Mathematics |
| written_exam_date | 2017-03-20 |
| written_exam_score | 21 |
| written_score_date | 2017-03-30 |
| oral_exam_date | 2017-03-25 |
| oral_exam_score | 10 |
| oral_score_date | 2017-03-25 |

Add a new student with id = 11 to the student table. The teacher knows only his last name: Barry. 

Don't use NULL when inserting this data.

```sql
INSERT INTO student (id, last_name)
VALUES (11, 'Barry')
```

Add info about Tom Muller's written math exam to this table. You know only the exam ID (17), the student ID (12), Tom's score on the written exam (12), the date of the written exam (October 14, 2018), and when the exam was scored (October 16, 2018).

```sql
INSERT INTO exam (id, student_id, written_exam_score, written_exam_date, written_score_date)
VALUES (17, 12, 12, '2018-10-14', '2018-10-16')
```

In the database, the teacher stores the student name Laura Donna Ross (id = 10). However, Laura doesn't have a middle name; Donna is a mistake. Correct this mistake by updating the data.

```sql
UPDATE student
SET middle_name = NULL
WHERE id = 10
```

Some students didn't receive any scores on a written exam—the teacher forgot to enter their scores. Correct this mistake by assigning all missing written exam values a score of 43.

```sql
UPDATE exam
SET written_exam_score = 43
WHERE written_exam_score IS NULL
```

A group of students weren't scored on an oral exam, so the exam was canceled. The results from this exam have to be removed. Write a query to remove these records.

```sql
DELETE FROM exam
WHERE oral_exam_score IS NULL
```

Assign a score of 3 points to all students who do not have an oral English exam score date.

```sql
UPDATE exam
SET oral_exam_score = 3
WHERE oral_score_date IS NULL
  AND subject = 'English'
```

Our university is not accredited to conduct exams in Spanish and Mathematics. Delete all records for these exams.

```sql
DELETE FROM exam
WHERE subject = 'Spanish' OR subject = 'Mathematics'
```

The oral exams with no date assigned take place four days after the written_exam_date. Update the data.

```sql
UPDATE exam
SET oral_exam_date = written_exam_date + INTERVAL '4' DAY
WHERE oral_exam_date IS NULL
```

It's time to fix another mistake in our database. Somebody mistook the first name of the student whose ID is 6 for his last name. See if you can use the example query to correct this.

```sql
UPDATE student
SET first_name = last_name, last_name = first_name
WHERE id = 6
```

The university wants to have a list of all oral exams in which students got more than 20 points. They want it in a new table, report_good_oral_exams.

Use the id, student_id, and subject columns from the exam table to put data into exam_id, student_id, and subject columns in the report_good_oral_exams table.

```sql
INSERT INTO report_good_oral_exams
SELECT id, student_id, subject
FROM exam
WHERE oral_exam_score > 20
```

This time, the university needs a list of average results of written and oral exams in each subject (from the exam table).

The new table, report_average_scores consists of the following columns: subject, avg_written_exam_score, and avg_oral_exam_score (which are of type DECIMAL(4,2)).

Help the university insert data into the report_average_scores table.

```sql
INSERT INTO report_average_scores
SELECT subject, AVG(written_exam_score), AVG(oral_exam_score)
FROM exam
GROUP BY subject
```

## Working with DEFAULT values

_author_

| column_name | sample value |
| ---- | ----|
| id | 1 |
| first_name | Marian |
| last_name | Kowalsky |
| photo | imgs/MK.jpg |
| create_timestamp | 2018-09-20 21:13:02.3 |
| is_active | t |

_post_

| column_name | sample value |
| ---- | ----|
| id | 1 |
| author_id | 10 |
| title | Incredible history |
| text | Archaeologists in Egypt… |
| modified_timestamp | 2018-10-12 12:15:30.2 |

Let's add the data for Cindy Barry into the author table. She should have an ID of 2, a photo path set to 'imgs/cindy.jpg', and a registration date of '2018-08-27'.

We don't know if Cindy wants to have an active account yet, so we'll leave this as a DEFAULT value.

```sql
INSERT INTO author
VALUES (2, 'Cindy', 'Barry', 'imgs/cindy.jpg', '2018-08-27', DEFAULT)
```

Martin Williams (id = 4) registered to join the site on 2018-09-30. Insert his data with default values for the photo and is_active columns.

```sql
INSERT INTO author
VALUES (4, 'Martin', 'Williams', DEFAULT, '2018-09-30', DEFAULT)
```

The author with ID of 7 wrote his first short post:

'Our company sold about 23 percent more magazines this year than it did 2 years ago.'

The title is

'Increased magazine sales'

and the post ID is 5. Insert this data into the post table using the default date, which is the current date and time.


```sql
INSERT INTO post
VALUES (5, 7, 'Increased magazine sales',
  'Our company sold about 23 percent more magazines this year than it did 2 years ago.',
  DEFAULT)
```

The author Alan Hillary (id = 3) would like to set the is_active value to the default and change the path for his photo to imgs/alan3.jpg.

```sql
UPDATE author
SET is_active = DEFAULT, photo = 'imgs/alan3.jpg'
WHERE id = 3
```