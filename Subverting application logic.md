# Subverting application logic
<br>

## Example
<br>

Consider an application that lets users log in with a username and password. If a user submits the username `wiener` and the password `bluecheese`, the application checks the credentials by performing the following SQL query:

`SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`

If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

Here, an attacker can log in as any user without a password simply by using the SQL comment sequence `--` to remove the password check from the `WHERE` clause of the query. For example, submitting the username `administrator'--` and a blank password results in the following query:

`SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`

This query returns the user whose username is `administrator` and successfully logs the attacker in as that user.

<br>

## Goal
<br>
login specific user with **any** password.

## Analytics
<br>

sql can separate to these :
```sql
SELECT * FROM users WHERE username = '  -- auto generated, cannot modify
wiener -- user input 
' AND password = ' -- auto generated, cannot modify
bluecheese -- user input
' -- auto generated, cannot modify

```

### sql should be this:

```sql

SELECT * FROM users WHERE username = 'wiener' -- AND password = 'bluecheese'

```

Try to add `--` after `username = 'wiener'`, in this case, `password` no longer have any effect to login.


##  exercise 
[[lab2]]

