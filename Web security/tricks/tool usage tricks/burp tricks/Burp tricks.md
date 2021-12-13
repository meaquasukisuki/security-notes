# Burp tricks

## burp修改response包
burp proxy抓到包后可以修改response包，并发出去。可能可以过掉登录。


## burp socks5 访问内网服务器

前提： 假设`12server4`是已经被渗透的受害主机。 简称受害机，```12server5```是要存在想要资源的机器， 需要通过`12server4`访问`12server5`.


虚拟机`12server4` 添加一个网卡，属于VMnet19内网
虚拟机`12server5`设置网络，属于VMnet19
`12server4`：
![[Pasted image 20211212174652.png]]
`12server5`：
![[Pasted image 20211212174722.png]]


各自ip（内网）：
`12server4`:
![[Pasted image 20211212175049.png]]

`12server5`:
![[Pasted image 20211212175146.png]]


受害机访问资源: 
已知6588端口开放着，不花费功夫验证12server5有什么端口开放了。验证被渗透的12server4可以访问内网资源即可
![[Pasted image 20211212175258.png]]



下一步是让host机（不在内网的主机）也能访问到内网资源。通过socks5代理

受害机使用ew for Windows开启socks5(1080 port),查了一下也有ew for Linux版本的。 全名叫做`earthworm`,现在已经停止更新了。
官网： http://rootkiter.com/EarthWorm/
当然，Linux即使用ssh也能实现类似效果，，， 比较麻烦就是了


1. 在`12server4`装ew
2. 1080端口开启socks 
 ![[Pasted image 20211212184836.png]]
```shell
ew_for_Win.exe -s ssocksd -l 1080
```

3. 本机burp监听，需要填写的ip是**非内网ip！（可以和外网通信）**
 ![[Pasted image 20211212192541.png]]
 4. 本机访问内网资源测试
![[Pasted image 20211212185831.png]]
关闭被渗透受害机防火墙：成功访问
![[Pasted image 20211212192628.png]]![[Pasted image 20211212192653.png]]


## burp上游代理访问内网

??????????


