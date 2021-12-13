# IIS6.0 PUT漏洞任意文件上传

前提条件：
1. 开启Webdav ![[Pasted image 20211212233247.png]]
2. 对应网站开启写权限  ![[Pasted image 20211212233351.png]]

PS: 什么是webdav？  简单来说就是提供网站并发和其它功能的extensions，需要开启它才能确保网站是可读/可写的而不是read-only。
 **WebDAV** (**Web Distributed Authoring and Versioning**) is a set of extensions to the [Hypertext Transfer Protocol](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol "Hypertext Transfer Protocol") (HTTP), which allows [user agents](https://en.wikipedia.org/wiki/User_agent "User agent") to collaboratively author contents _directly_ in an [HTTP web server](https://en.wikipedia.org/wiki/Web_server "Web server") by providing facilities for [concurrency control](https://en.wikipedia.org/wiki/Concurrency_control "Concurrency control") and [namespace operations](https://en.wikipedia.org/wiki/Namespace "Namespace"), thus allowing [Web](https://en.wikipedia.org/wiki/World_Wide_Web "World Wide Web") to be viewed as a _writeable, collaborative medium_ and not just a read-only medium.(https://en.wikipedia.org/wiki/WebDAV#cite_note-FOOTNOTEWhiteheadGoland1999293-1) WebDAV is defined in [RFC](https://en.wikipedia.org/wiki/RFC_(identifier) "RFC (identifier)") [4918](https://datatracker.ietf.org/doc/html/rfc4918) by a [working group](https://en.wikipedia.org/wiki/Working_group "Working group") of the [Internet Engineering Task Force](https://en.wikipedia.org/wiki/Internet_Engineering_Task_Force "Internet Engineering Task Force") (IETF).(https://en.wikipedia.org/wiki/WebDAV#cite_note-FOOTNOTEWhitehead199834-2)
ref：https://en.wikipedia.org/wiki/WebDAV

漏洞复现详细过程
1. 下载安装Windows server 2003虚拟机。![[Pasted image 20211213095011.png]]
2. 查看是否开启webdav，对应网站是否都开启了读/写功能![[Pasted image 20211213095118.png]]![[Pasted image 20211213095154.png]]

3. 查看虚拟机ip（需要和本机在同一subnet，如果绑定了ip务必注意），**本机**修改对应hosts 。![[Pasted image 20211213095413.png]]![[Pasted image 20211213095438.png]]![[Pasted image 20211213095502.png]]
4. 都改好后查看能否访问![[Pasted image 20211213095536.png]]
5. burp首页抓包，**OPTIONS**看看http request method是否支持PUT。 ![[Pasted image 20211213095836.png]]
6. 上传一句话小马，先必须是`.txt`形式，要么上传不上去![[Pasted image 20211213100314.png]]![[Pasted image 20211213100501.png]]
7. 再用MOVE方法把它改成`.asp`形式小马![[Pasted image 20211213101047.png]]![[Pasted image 20211213101123.png]]
8. 访问，可以用菜刀之类的连上这个小马：![[Pasted image 20211213101215.png]]