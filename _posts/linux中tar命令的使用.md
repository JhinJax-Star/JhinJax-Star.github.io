# tar简介
在linux中tar是一个常用的工具，用于打包和解压文件，全称是tape archive。
它能够将一组文件和目录打包成单个归档文件，也可以从归档文件中提取出文件和目录

## 参数列表：
![3082583-20240905193239263-1881472081](https://github.com/user-attachments/assets/e2cbd4fd-9e4e-4243-99d8-772cef3da6a1)

## 使用实例介绍
### 1.要创建一个归档文件，可以使用参数 -c 和 -f ，然后指定归档文件名
```
例如要将/home/cyber 目录打包成一个文件，可以运行以下命令：
tar -cf document.tar /home/cyber
```
###  2.解包归档文件，可以使用 -x 和 -f ，然后指定归档文件名
```
例如要将document.tar解包到当前目录，可以运行以下命令
tar -xf document.tar
```
### 3.压缩归档文件，tar命令可以与压缩工具一起使用，以创建压缩的归档文件
```
常见的压缩选项有 -z （使用gzip压缩） 和 -j （使用bzip2压缩） 
例如，要创建一个gzip压缩的归档文件，可以运行以下命令：
tar -czf document.tar.gz /home/user/documents

这将创建一个名为documents.tar.gz 的压缩归档文件
```

### 4.列出归档文件内容
```
可以使用 --list 参数来列出归档文件中的内容，而无需实际提取它们
tar --list -f document.tar

这将显示出documents.tar 中包含的所有文件和目录列表
```

### 5.特殊的性质
```
如果某个用户的目录下存在一个可执行的tar，且这个tar具有root的权限
那么，用这个tar软件去压缩一个只有root才能查看的文件，再解压出来
则可以消除权限问题，我们控制的某个用户此时也可以查看该文件

注意：
压缩时用的是 ： ./tar 此时表示的是使用该用户目录下的tar软件进行压缩
解压时用的是： tar 此时表示的是系统路径中的tar软件，而不是该目录下的tar软件
```




