# IIS 6.0解析漏洞 

## 文件名解析漏洞 ：;相当于截断符
例：
`shell.asp;.jpg`会以`asp`文件执行，分号相当于截断符
另：.cdx,.cer,asa扩展名也会解析成asp

![[Pasted image 20211213105522.png]]

cer:
![[Pasted image 20211213105624.png]]
![[Pasted image 20211213105639.png]]

原因： ![[Pasted image 20211213110141.png]]

## 文件夹解析漏洞

`*.asp/xxxxx` xxxxx都按照`asp`文件解析。
例： ![[Pasted image 20211213110903.png]]![[Pasted image 20211213110923.png]]![[Pasted image 20211213110933.png]]


