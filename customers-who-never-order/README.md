# Customers Who Never Order

Suppose that a website contains two tables, the `Customers`
table and the `Orders` table. Write a SQL query to find all
customers who never order anything.

Table `Customers`:

+----+-------+
| Id | Name  |
+----+-------+
| 1  | Joe   |
| 2  | Henry |
| 3  | Sam   |
| 4  | Max   |
+----+-------+

Table `Orders`:

+----+------------+
| Id | CustomerId |
+----+------------+
| 1  | 3          |
| 2  | 1          |
+----+------------+

Using the above tables as an example, return the following:

+-----------+
| Customers |
+-----------+
| Henry     |
| Max       |
+-----------+

## Hints

1. Which join type gets you there?

## Solutions

### Ye old LEFT JOIN

Here we use a `LEFT JOIN` to grab all rows from the left side of
the join and then look for a mssing right side of the join.

```sql
SELECT c.Name Customers
FROM Customers c
LEFT JOIN Orders o ON c.Id = o.CustomerId
WHERE o.CustomerId IS NULL;
```
