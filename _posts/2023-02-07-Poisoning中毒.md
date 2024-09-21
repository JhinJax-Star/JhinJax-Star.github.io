## Poisoning是一种漏洞，可以通过利用这个漏洞，来执行一些php命令

如果可以在靶机中找到一个可读文件，例如access.log或是error.log，一般是/var/log/apache2/access.log。
那么可以利用log中毒来执行其他的命令

例如:
```
在浏览器中输入
http://test.com/test.php?view=/var/www/html/test/.././.././.././.././.././var/log/apache2/access.log&cmd=id
```

就可以执行id命令
当然也可以执行其他的命令，像reverse-shell、wget等命令，然后就可以侵入主机了
用wget获取文件最好用  -O 来指定输出文件目录，方便chmod和执行。
