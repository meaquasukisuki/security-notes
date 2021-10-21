# Blind SQL injection with conditional responses

Lab: Blind SQL injection with conditional responses
https://ac541f531e9e53a3c07f85b900d10036.web-security-academy.net/
<br>

This lab contains a [blind SQL injection](https://portswigger.net/web-security/sql-injection/blind) vulnerability. The application uses a tracking cookie for analytics, and performs an SQL query containing the value of the submitted cookie.

The results of the SQL query are not returned, and no error messages are displayed. But the application includes a "Welcome back" message in the page if the query returns any rows.

The database contains a different table called `users`, with columns called `username` and `password`. You need to exploit the blind [SQL injection](https://portswigger.net/web-security/sql-injection) vulnerability to find out the password of the `administrator` user.

To solve the lab, log in as the `administrator` user.




inject point: cookie TrackingId

```sql
select xxx from xxx where trackingId = '
somethingUserInput
'...........
```

```sql
select xx from xxx where trackingid = '
something' and '1' = '1' -- 
'
```
This response page should return `Welcome back`.
![[Pasted image 20211020165806.png]]

<br>
<br>


```sql
select xx from xxx where trackingid = '
something' and '1' = '2' -- 
'
```
This response page should not return `Welcome back`.
![[Pasted image 20211020165856.png]]


<br>

check if have database called `user`
![[Pasted image 20211020192555.png]]

check if have `administrator` in users
![[Pasted image 20211020192730.png]]

check if have one password

![[Pasted image 20211020192956.png]]

use burp intruder to get the password length
![[Pasted image 20211020193351.png]]
![[Pasted image 20211020193449.png]]
![[Pasted image 20211020193518.png]]

Result : password length is 20.
![[Pasted image 20211020193649.png]]

<br>

Find out password

![[Pasted image 20211020193855.png]]

Result: ![[Pasted image 20211020194626.png]]