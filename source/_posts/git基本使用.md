---
title: git基本使用
categories: git基本使用
tags: git基本使用
date: 2023/3/18
cover: https://tangjiusheng.com/d/file/image/20190724/1563935209915315.jpg
---

### **初次使时的全局设置：**

git config --global user.name '新用户名'

git config --global user.email '新邮箱地址'

git config --global --list（此代码可以显示出当前的”用户名“和”邮箱地址“）



初始化仓库：git init

把本地文件放到暂存区：git add .(点表示所有的文件)

把本地文件放到本地仓库里面：git commit -m '提交~~~网站'

`git commit -m ''规范：`

```bash
feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style： 格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动
```

链接远程仓库：git remote add origin 仓库地址(error:remote origin already exists.已经链接到了远程仓库，跳过此步即可)

把本地仓库的文件推送到远程仓库 push：git push -u origin master（出现错误的主要原因是gitee(github)中的README.md文件不在本地代码目录中此时我们要执行git pull --rebase origin master命令README.md拉到本地，然后执行git push origin master）

码云部署发布静态网站：

(1)在当前仓库中，点击“服务”菜单

选择 Gitee Pages

(2)点击“启动”

(3)会生成网站网址（也可以用草料二维码把网址生成二维码）



查看当前仓库对应的远程仓库地址：get remote -v

修改仓库对应的远程仓库地址：git remote set-url origin 仓库地址

### 批量删除gitee远程库文件

git config user.name			查看git账户

git config user.email			查看git邮箱

1、先将远程库拉回来，命令如下：git clone “远程库地址”
2、删除所有.o文件，命令如下git rm *.o -r //删除.o文件
3、提交变化到暂存区命令：git add . （不要忘了add后面的空格.）
4、提交到库，命令如下：git commit -m “clear”
5、提交到远程库：git push origin master此时可以看到远程库.o文件全部删除

$ ssh-keygen -t rsa -C "git邮箱"				生成ssh密钥

git checkout -b 分支名			新建分支

### 分支的创建、提交与合并

前提：已经初始化了自己的仓库并且已经把自己的项目框架提交到gitee/github仓库上面了

#### 1.创建新的分支

在项目根目录中

git checkout -b tabbar	(其中tabbar为分支名)

附：git branch（可以查看当前项目中的所有的分支）

#### 2.分支的提交与合并

1.将本地的tabbar分支进行本地的commit提交

git add .

git commit -m "完成了  tabbar  的开发"

2.将本地的tabbar分支推送到远程仓库进行保存

git push -u origin tabbar

3.将本地的tabbar分支合并到本地的master分支

git checkout master	//将分支切换到mater主分支上，可以通过git branch命令查看当前所在的分支

git merge tabbar

git push  （更新master主分支）

4.删除本地的tabbat分支（注意：删除时，不能处在要删除的分支上）

git branch -d tabbar

### 分支的查看

```bash
git branch
```

> 列出本地已经存在的分支，并且当前分支会用*标记

```bash
git branch -r
```

> 查看远程版本库的分支列表

```bash
git branch -a
```

> 查看所有分支列表（包括本地和远程，remotes/开头的表示远程分支）

```bash
git branch -v
```

> 查看一个分支的最后一次提交

```bash
git branch --merged
```

> 查看哪些分支已经合并到当前分支

```bash
git branch --no-merged
```

> 查看所有未合并工作的分支

### gitee多成员并行开发

例如：小王接到了一个项目，独自开发了某个功能后，小李又加入到了这个项目开发中，小李就克隆gitee中的项目仓库，也就是说在小王开发后的基础上再进行开发，与此同时小王也在开发其他的功能模块。一段时间后，小李负责的功能模块开发完毕，提交了代码更新了仓库。又过了一段时间，小王也开发完毕准备提交代码到远程仓库时，发现提交时出现错误，原因是此时的仓库代码已经被小李更新过了，仓库的版本号已经升级了，此时小王就需要将远程版本库与本地更新，即pull远程项目，此机制是保证本地提交之前和远程版本库是一致的，这样就不会覆盖他人的修改了。即输入**git pull origin master**命令，再执行提交代码，**git push origin master**。

### git常用命令

1、查看命令：ll

2、查看当前目录下有什么东西：ls

3、列出全部文件：ls  -a（ll  -a）

4、列出详细信息：ls  -l

5、去往某个地方：cd  文件夹名称

6、回到最开始的位置：cd  ~

7、回到上一级目录：cd  ..

8、去往根目录：cd  /

9、新建文件：vim  文件名.文件类型

10、切到命令模式：ESC

11、复制光标所在行的代码：(在光标模式下按)yy(复制)p(粘贴):wq(保存并退出):q!(强制退出不保存)shift+z(回到最初位置)

12、查看文件：cat  文件名

13、查看文件并显示行号：cat  -n  文件名

14、空行不计入行号：cat  -b  文件名

15、所有空行合并为一行：cat  -s  文件名

16、创建新文件夹：mkdir  文件名

17、显示行号：set  nu

18、取消显示行号：set  nonu

19、命令模式（按i可进入）编辑模式（按ESC可回到命令模式）

20、0(零)将光标调到行首；shift+$将光标跳到行尾；（：数字）将光标跳到某一行

21、替换某个数据：（:s /需要替换的数据/替换为）

22、替换一行：（:s  /需要替换的数据/替换为/g）

23、替换全部：（:%s  /需要替换的数据/替换为/g）

24、命令模式下：(u)还原

25、当前状态信息：git  status

26、查看文件最后一行：tail  -n  1  文件名

27、删除暂存区内的文件：git  rm  --cashed  文件名（git  add. 后文件就进入了暂存区）

28、查看日志信息：git  log

29、查看精简版的日志信息：git reflog

30、回到修改前的某个历史版本：git  reset  --hard  精简版本号