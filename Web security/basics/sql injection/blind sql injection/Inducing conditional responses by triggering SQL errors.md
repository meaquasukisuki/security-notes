# Inducing conditional responses by triggering SQL errors

Return data or not doesn't affect response results.
But we can try to let database throw an error, based on these info, we can get needed information.


lab : This lab contains a [blind SQL injection](https://portswigger.net/web-security/sql-injection/blind) vulnerability. The application uses a tracking cookie for analytics, and performs an SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and the application does not respond any differently based on whether the query returns any rows. If the SQL query causes an error, then the application returns a custom error message.

The database contains a different table called `users`, with columns called `username` and `password`. You need to exploit the blind [SQL injection](https://portswigger.net/web-security/sql-injection) vulnerability to find out the password of the `administrator` user.

To solve the lab, log in as the `administrator` user.


Try 1/0 error 
![[Pasted image 20211020195644.png]]

find out password length:
![[Pasted image 20211020200727.png]]

length : 20
![[Pasted image 20211020200808.png]]

find out password
![[Pasted image 20211020202801.png]]

<br>

Need to find a way quickly find out website's type,version.
Maybe sqlmap works.
