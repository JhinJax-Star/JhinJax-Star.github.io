## John Ripper——开膛手John，强力的hash暴力破解软件
### 0.先要破解hash就得先识别hash的类型

一般用hash-id.py的python脚本来自动识别hash

操作：
```
wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
python3 hash-id.py

保留这个shell窗口用于识别hash
```

看看该格式对应john里的格式：
```
john --list=formats | grep xxx

xxx即识别出来的格式，例如md5
```

查出来后有很多的关于该格式的类型，一般处理标准类的hash都要在前面加前缀raw

例如：格式为raw-MD5

### 1.基本类（像md5、sha1、sha256、whirlpool之类的）

操作：

查出格式后就传入字典、传入格式（例如raw-MD5）、传入带破解hash

```
john --wordlist=/usr/share/wordlist/rockyou.txt --format=格式  hash_to_crack.txt
```

### 2.破解windows类的hash值

NThash/NTLM是现代windows存储用户和服务密码的hash格式，以前的版本为LM，新技术下推出的为NT，故也为NT/LM。

这类的hash值丢入hash-id.py中识别不了，只能自行判断

操作：

```
john --wordlist=/usr/share/wordlist/rockyou.txt --format=NT hash_to_crack.txt
```

### 3.从/etc/shadow破解用户名以及密码

/etc/shadow是linux上储存密码hash的文件

破解密码可以先用john自带的unshadow将其转化为hash

在用john暴力破解

例如看到文件是这样的

```
root:x:0:0::/root:/bin/bash
root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZagtGwpSQhb3F2DOJtnHrvk7HI2ma4GsuioHp8sm3LJiRJpKfIf7lZQ29qgtH17Q/JDpYM/:18576::::::
```

那么就将 root:x:0:0::/root:/bin/bash 写入一个新的txt文件，例passwd.txt

将  root:$6$Ha.d5nGupBm29pYr$yugXSk24ZljLTAZZagtGwpSQhb3F2DOJtnHrvk7HI2ma4GsuioHp8sm3LJiRJpKfIf7lZQ29qgtH17Q/JDpYM/:18576::::::  这一串写入一个新的txt文件，例如shadow.txt

然后

```
unshadow passwd.txt shadow.txt > shadow_hash.txt
john --wordlist=/usr/share/wordlist/rockyou.txt --format=sha512crypt shadow_hash.txt
```

即可破解出用户名和名下密码

### 4.Single crack模式——以社工的方式破解hash密码

一些情况下会面临解单词重组的密码：

  马库斯1、马库斯2、马库斯3（等）
  
  MArkus， MARkus， MARKus （等）
  
  马库斯！， 马库斯$， 马库斯* （等等） 
  
这种是字符长度不变，但字符内部有变的类型

例子：

给定用户名Joker，给定hash值：7bf6d9bb82bed1302f331fc6b816aada

先分析hash值的格式，例为md5

写入一个新的txt文件，例hash_to_crack.txt，内容为：
```
Joker：7bf6d9bb82bed1302f331fc6b816aada
```
接着：
```
john --single --format=raw-md5 hash_to_crack.txt
```
就可以得到Joker用户对应的密码

### 5.破解zip类、rar类、ssh密码类

对于zip类的解压密码破解：
```
zip2john xxx.zip > hash.txt

john --wordlist=/usr/share/wordlist/rockyou.txt hash.txt
```
即可得到zip的解压密码，接着 unzip xxx.zip，输入密码就行



对于rar类的解压密码破解：
```
rar2john qwe.rar > hash.txt
john --wordlist=/usr/share/wordlist/rockyou.txt hash.txt
```
即可得到rar的解压密码，接着 rar x qwe.rar，输入密码就行

没有rar就 apt install rar -y 安装rar

 

对于ssh密码类的破解：
```
ssh2john id_rsa > id_rsa_hash.txt
john --wordlist=/usr/share/wordlist/rockyou.txt id_rsa_hash.txt
```
即可获得ssh的密码
