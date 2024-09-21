# 试过很多的方法，但发现就这个适用于我

## 1.先找到想上传的文件夹，进入该文件夹，右键gitbash-here

## 2.在gitbash界面输入
```
git clone https://XXXX.git

即你要上传的仓库链接，一定得是公开的
```

## 3.接着你会在本地文件夹里看到新建了一个文件夹，把所有你想上传的文件或文件夹都复制到新的文件夹里

## 4.gitbash界面cd到新文件夹目录下

## 5.
```
1 git add .
把所以文件都添加进缓存
2 git commit -m "你要添加的版本信息"
3 git push -u origin master
```

## 6.最后刷新github网页就行了
