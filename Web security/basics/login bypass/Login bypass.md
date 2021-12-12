# Login bypass



## cookie 
登录代码：

```php

<?php
	include 'init.php';
	if($_COOKIE['username']){
		echo "登录成功【{$_COOKIE['usernme']}】"
	}
	?>
	<meta charset="UTF-8">
	<form method='post'>
		<label>帐号：</label><input type='text' name='username'><br>
		<label>密码：</label><input type='password' name='password'><br>
		<input type='submit' value='登录' name='submit'>
	</form>
		
?>


```

这段代码只是验证是否存在cookie上username字段是否存在判断用户是否登录、
bypass的情况抓包，随便加一个```username```字段就可以绕过登录。

**cookie可以被外部控制**

修复方法： 不要用cookie做身份验证

## session

