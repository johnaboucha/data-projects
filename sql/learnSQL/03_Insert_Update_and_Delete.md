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


