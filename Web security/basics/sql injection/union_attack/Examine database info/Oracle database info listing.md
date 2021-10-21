# Oracle database info listing

## How to get oracle database's version?

Take a look at sql injection cheat sheet.

https://portswigger.net/web-security/sql-injection/cheat-sheet

![[Pasted image 20211019192855.png]]


<br>

Oracle database must have `from` keyword.
<br>

e.g:

`select null,null` not working here. Must use `select null,null from dual`.


Exercise : [[lab6]]