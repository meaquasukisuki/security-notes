#  SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

***link:*** https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data
<br>

## description
This lab contains an [SQL injection](https://portswigger.net/web-security/sql-injection) vulnerability in the product category filter. When the user selects a category, the application carries out an SQL query like the following:

`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

To solve the lab, perform an SQL injection attack that causes the application to display details of all products in any category, both released and unreleased.

## solving process

1. Use burpsuite capture requests.![[Pasted image 20211019143140.png]]
2. Send request to repeater, and add  `' and 1 = 1 -- `.![[Pasted image 20211019143918.png]]
3. press ctrl + u to url encode added sql string,and send the request.  ![[Pasted image 20211019144135.png]]


Lab related to : [[Retrieving hidden data]]
