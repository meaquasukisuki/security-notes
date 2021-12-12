# Path traversal

找到一个Springboot综合靶场WebGoat
配置了两套(docker和idea直接启动debug的版本)，配置过程不再赘述


进入path traversal章节,最后一章上传图片要求zip file的太怪了就没写上来。

![[Pasted image 20211201205218.png]]

path traversal漏洞核心是利用文件上传时，需要在服务器上有一个上传路径。该路径可能是直接由前端输入拼接而来，不经过滤或绕过过滤，而让客户client访问/修改到服务器上的其它文件。

第一关的路径是由固定字符和full name拼接而来，并且无任何过滤，所以full name处输入 ``` ../test``` 即可通关。

代码体现：

PathTraversal.html:
![[Pasted image 20211201210053.png]]


ProfileCallback应该是在某个JS里面，它不是直接调用的接口。 寻找path_traversal.js
![[Pasted image 20211201210341.png]]
该函数调用profile-picture接口。

接下来寻找profile-picture，profile-upload接口在哪。

后端接口在
![[Pasted image 20211201210815.png]]



getProfilePicture分析：
![[Pasted image 20211201211310.png]]
->
![[Pasted image 20211201211336.png]]
response返回图片的base64形式
->
前端渲染img：
![[Pasted image 20211201211454.png]]



form action post上传图片使用uploadHandler接口：

![[Pasted image 20211201211705.png]]

->
![[Pasted image 20211201212227.png]]
![[Pasted image 20211201212259.png]]
路径uploadDirectory的后半部分是可变（拼接）的，漏洞正是出现在此处
再往后就是创建文件，写入文件的过程了。

solveIt的满足条件：parent file（folder）以PathTraversal结尾
![[Pasted image 20211201212801.png]]

所以只把图片上传到比原来更向上的一级目录就可以通过了。


## Path traversal 02

结果和payload
![[Pasted image 20211201222008.png]]

类似1的代码分析，不过给输入加上了过滤：

![[Pasted image 20211201222204.png]] 会过滤掉``` ../ ``` 一次。所以需要双写，payload：``` ....// ``` 过滤掉内部的，留下外部的成功构成有效payload


## Path traversal 03
代码框架基本一致，改变了过滤和上一次不同。

黑盒：

![[Pasted image 20211201225705.png]]


看起来好像不能用full Name来弄了   路径取决于文件名   看起来文件名没有过滤。

一种方法是直接改上传文件的文件名，另一种方法就是post的时候改包：
![[Pasted image 20211201230002.png]]
->
![[Pasted image 20211201230612.png]]

forward request，成功
![[Pasted image 20211201230719.png]]

代码：

和猜测的一样，跟full name无关了，和文件名有关，execute函数还是那个，传入参数不同
![[Pasted image 20211201230847.png]]


## Path traversal 04

![[Pasted image 20211201235402.png]]
找个名叫``` path-traversal-secret ``` 的文件，然后弄个flag，估计和这个文件有关。

黑盒：

点show random picture后 
network tab抓包：
![[Pasted image 20211201235612.png]]
观察response header：
![[Pasted image 20211201235734.png]]
我们是要找个secret img文件 ，先扔到burp里看看：
![[Pasted image 20211201235957.png]]
```7.jpg```这个file肯定是有的，但是response status code是404，观察location，它是自动加了个``` .jpg ```，所以request里就不需要加``` .jpg ``` 了。
下面的response body info里看一下，把这个directory下的文件目录给显示出来了， cat下面只有1-10 jpg文件，没有想要的。
那寻找一下上一级，看看能不能成
![[Pasted image 20211202000428.png]]
被拦截了。试试url encode之后咋样：
![[Pasted image 20211202000539.png]]看目录还是没有
在上一级： ![[Pasted image 20211202000645.png]]
看response info，目标在这一级目录下。

``` ../../path-traversal-secret``` url encode一下，发送请求：
![[Pasted image 20211202000842.png]]
成功找到了。 flag是把用户名SHA-512加密。



代码：

前端：
![[Pasted image 20211202091506.png]]

![[Pasted image 20211202091536.png]]

random-picture对应的后端接口：

![[Pasted image 20211202091656.png]]

不能有 ``` .. ``` 和``` / ``` ，否则会直接400 Bad Request
![[Pasted image 20211202092108.png]]
cat picture是如果request parameter里面id为空，那么就从1-11中随机取一个数作为id。
如果不为空，则用request parameter里的id

![[Pasted image 20211202092555.png]]
![[Pasted image 20211202092420.png]]

如果cat picture的name包含```path-traversal-secret.jpg```，那么直接返回status code 200，response body是这个图片


如果id是随机取的，并且文件存在，返回status code 200，response body是这个图片
否则返回404，并且response body列出同级目录。

这样看思路就很明显了， 直接把id设置成```path-traversal-secret.jpg``` 就可以得到答案。















