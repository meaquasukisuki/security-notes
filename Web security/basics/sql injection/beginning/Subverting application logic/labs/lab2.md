# Lab2 : SQL injection vulnerability allowing login bypass
<br>

## description

<br>

This lab contains an [SQL injection](https://portswigger.net/web-security/sql-injection) vulnerability in the login function.

To solve the lab, perform an SQL injection attack that logs in to the application as the `administrator` user.

## solve process

<br>

1. Go into login page and use burp to capture login request.![[Pasted image 20211019150446.png]]![[Pasted image 20211019150524.png]]
2. Injecting points are shown above.  `csrf=4cvCLnetsZJn57zF7A5JbO0CIZlh89fD&username=ddsdsdsdsd&password=sddssdsdsd`
3.  Username input should be `adminstrator' --`
4.  Result ![[Pasted image 20211019151121.png]]![[Pasted image 20211019151438.png]]

<br>

[[Subverting application logic]]