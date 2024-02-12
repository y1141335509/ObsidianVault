
## 新建Github Repo
### 本地已有一个项目，如何将该项目新建到Github？
首先cd 到这个项目本地的根目录下，例如我有一个前端网站的项目，根目录为 `~/Users/my-project`。然后运行：
```git
git init
```

接着登录你的GitHub账号，创建一个repo（这里推荐你创建一个与你项目根目录文件夹相同名字的repo）

创建后，你会在GitHub上看到新建repo的https或者ssh链接。复制该链接。
然后在刚刚的terminal中运行：
```git
git remote set-url origin https://github.com/YOUR_USERNAME/YOUR_PROJECT_NAME.git
git add .
git commit -m 'your initial commit'
git push -u origin main
```
（注：最后一行命令也可能是`git push -u origin master`）
如果你遇到了下面的报错：`error: No such remote 'origin'`则说明`origin`还没有被创建，所以你需要运行：
```git
git remote add origin https://github.com/YOUR_USERNAME/YOUR_PROJECT_NAME.git
git add .
git commit -m 'your initial commit'
git push -u origin main
```






## Mac 通过terminal更改系统的git账号
```git
git config --global user.email "hello@world.com"
git config --global user.name "your_name"
```





## 合并当前branch到`main` branch上
假设你在testDB branch上。确保你把所有的改动都`push`到testDB上后

然后来到`main` branch
```git
git checkout main
```

然后确保`main` branch也是最新的
```git
git pull origin main
```

然后运行下面命令来把`testDB` branch上所有的改动带到`main` branch上：
```git
git merge testDB
```

最后运行下面的命令：
```git
git add .
git commit -m 'you message'
git push origin main
```

最后你可以在切换回`testDB` branch继续工作：
```git
git checkout testDB
```






