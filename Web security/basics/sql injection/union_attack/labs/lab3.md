# SQL injection UNION attack, determining the number of columns returned by the query
<br>

## description

This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. **The first step of such an attack is to determine the number of columns that are being returned by the query**. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing an [SQL injection UNION](https://portswigger.net/web-security/sql-injection/union-attacks) attack that returns an additional row containing null values.

## Analytics

The original sql should be: 
```sql
select * from table where catagory = '
Accessories 
'.........
```

add union select statement at end.

```sql
select * from table where catagory = '
Accessories' union select null,null.... --
'.........
```
## Solving process

1. Try: if table only have one column:![[Pasted image 20211019163216.png]] ,and url encode it,response is: ![[Pasted image 20211019163310.png]] **Not One.**
2. Try 2 nulls: ![[Pasted image 20211019163444.png]]
3. Try 3: ![[Pasted image 20211019163532.png]] ![[Pasted image 20211019163619.png]]

So the specific table have 3 columns.



<br>

related to topic:  [[union attacks]]

related lab: [[lab4]]