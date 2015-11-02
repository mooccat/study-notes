# Git学习
## 简介
+ Git是目前世界上最先进的分布式版本控制系统

## 安装Git
+ sudo apt-get install git(ubuntu)
+ 安装完成后，还需要最后一步设置，在命令行输入：
```
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
```

## 创建版本库
+ 选择一个合适的地方，创建一个空目录,进入目录
+ 通过git init命令把这个目录变成Git可以管理的仓库
> 瞬间Git就把仓库建好了，而且告诉你是一个空的仓库（empty Git repository），当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的，没事千万不要手动修改这个目录里面的文件，不然改乱了，就把Git仓库给破坏了。如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。

+ 把文件添加到版本库
>- 编写一个readme.txt文件，放到刚才创建的目录下
>- git add readme.txt把文件添加到仓库
>- git commit 把文件提交到仓库
>- `$ git commit -m "wrote a readme file"`
> ++简单解释一下git commit命令，-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。++
>- 为什么Git添加文件需要add，commit一共两步呢？因为commit可以一次提交很多文件，所以你可以多次add不同的文件.比如：
>```
> $ git add file1.txt
> $ git add file2.txt file3.txt
> $ git commit -m "add 3 files."
> ```

## 时间机穿梭
+` git status`命令可以让我们时刻掌握仓库当前的状态
+` git diff`查看修改内容

### 版本回退
+ `git log`：查看历史记录（如果嫌输出信息太多，看得眼花缭乱的，可以试试加上`--pretty=oneline`参数）
+ 在Git中，用HEAD表示当前版本，HEAD^ 表示上个版本，HEAD^^表示上上个版本，HEAD～100表示上100个版本
+ `git reset  --hard HEAD^`回退到上一个版本
+ `git reflog`记录命令

### 工作区和暂存区
+ 工作区（Working Directory）：就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区：
+ 版本库（Repository）：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

### 撤销修改
+ 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`
+ 你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD file`，就回到了场景1，第二步按场景1操作。
+ 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 删除文件
+ `git rm`删掉，并且`git commit`
+ 删错了恢复到最新版本:`git checkout -- file.txt`

## 远程仓库
+ 创建SSH Key：`$ ssh-keygen -t rsa -C "youremail@example.com"`
+ 关联到github：`$ git remote add origin git@github.com:michaelliao/learngit.git`
+ 推送到github：`$ git push -u origin master`
+ 把本地master分支的最新修改推送至GitHub：`$ git push origin master`
+ 克隆远程：`$ git clone git@github.com:michaelliao/gitskills.git`

## 分支管理
+ 创建合并分支dev：`$ git checkout -b dev`（git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：`$ git branch dev`、`$ git checkout dev`)
+ 查看当前分支：`git branch`
+ 合并分支dev： `$ git merge dev`
+ 删除分支dev：`$ git branch -d dev`
+ 分支合并图：`git log --graph`
+ 强制禁用Fast forward模式:`$ git merge --no-ff -m "merge with no-ff" dev`
+ 把当前工作现场“储藏”起来，等以后恢复现场后继续工作：`$ git stash`
+ 查看存储的工作现场：`git stash list`
+ 恢复存储的工作现场：`git stash apply`（恢复后删除：`git stash drop`）
### Feature分支：
> 每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。