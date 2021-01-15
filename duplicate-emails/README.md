# Duplicate Emails

|Id  | Email   |
|1   | a@b.com |
|2   | c@d.com |
|3   | a@b.com |

## Hints

1. Well you shouldn't be reading this. But if you are you should regroup and
   try having a good day.

## Solutions

### Group by having

```sql
SELECT p.Email FROM Person p
GROUP BY p.Email
HAVING COUNT(p.Email) > 1;
```
