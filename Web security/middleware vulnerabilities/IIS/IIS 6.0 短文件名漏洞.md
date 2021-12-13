# IIS 6.0短文件名漏洞


短文件名命令    `dir /x`
![[Pasted image 20211213112420.png]]

放在Windows系统的文件存在短文件名

例如： `aaaaaaaaaaaaaaaaaaaaaaaaaaa.txt`  `aaaaaaaaaaaaaaaaaaaaaaaaaaa`叫做前缀,`txt`叫做后缀

前缀只显示前六位字符，后面的被`~1`截断。![[Pasted image 20211213115648.png]]
后缀只显示前三位字符![[Pasted image 20211213115719.png]]

非字母/数字的文件名or文件夹名一定有短文件名

多个后缀名的时候短文件名取最后一个 ![[Pasted image 20211213120256.png]]




基于以上特征，可以猜出短文件名。

构造猜解语句： `http://upload.moonteam.com/*~1*/a.aspx`

`*~1*`的意义是某个短文件名。   `111~1.txt`匹配，`222~1.asp`也匹配 ，而且需要再往下一级访问不存在的`a.aspx`，要不然不能看出区别。
如果没有再往下一级访问`a.aspx`,无论文件存在不存在都是返回404页面。
有访问下一级`a.aspx`，如果存在短文件名对应的文件，返回404，如果不存在短文件名对应的文件，返回400 bad request。

存在222.....：
![[Pasted image 20211213121311.png]]![[Pasted image 20211213121324.png]]

不存在kokoko......：
![[Pasted image 20211213121349.png]]![[Pasted image 20211213121400.png]]

根据状态码不同，可以用burp intruder猜解。也有专门针对这种短文件名的exp工具 `iis_shortname_Scan.py`.










