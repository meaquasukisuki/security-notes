# Linux Tricks

## File permission


![[Pasted image 20211209183948.png]]

例： ```ls -l```
![[Pasted image 20211209184055.png]]
会列出文件权限

```


rwx rwx rwx = 111 111 111
rw- rw- rw- = 110 110 110
rwx --- --- = 111 000 000

and so on...

rwx = 111 in binary = 7
rw- = 110 in binary = 6
r-x = 101 in binary = 5
r-- = 100 in binary = 4


```


所以chmod 777的意思是 把该文件/文件夹的权限修改为rwx rwx rwx

参考链接：
https://linuxcommand.org/lc3_lts0090.php