# Retrieving hidden data


## Example

Consider *https://insecure-website.com/products?category=Gifts*
Which will cause a sql selection statement:
```sql

SELECT * FROM products WHERE category = 'Gifts' AND released = 1

```

***released = 1***  is not simple. May have release 0,2,3........etc.
<br>

## Goal:
Get  all  released  `Gifts`.

<br>

## Analytics

The sql query could be separated as

```sql
	select * from products where category = '   -- auto generated,cannot modify
	Gifts  -- user input 
	'AND released = 1 -- auto generated,cannot modify
```
And these separate parts combined into one line sql query.
<br>

### Injection points

What we can modified is just  `Gifts`  in https://insecure-website.com/products?category=Gifts.
We need to find a way to modify the sql query into 

```sql
select * from products where category = 'Gifts' --
```
`-- ` comment out rest *useless* sql string.

## Exercise
[[Lab1]]














