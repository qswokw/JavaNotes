# git 命令

### 创建仓库


* **git init**

  ```
  Initialized empty Git repository in /Users/michael/learngit/.git/
  ```


 通过`git init`命令把当前目录变成Git可以管理的仓库 ，建议使用空目录。

### 提交到仓库

* **git add file1.txt**
* **git add file2.txt file3.txt**
* **git commit -m "add 3 files."**

-m  `-m`后面输入的是本次提交的说明 

 git add . ：他会监控工作区的状态树，使用它会把工作时的**所有变化提交**到暂存区，包括文件内容修改(modified)以及新文件(new)，但**不包括被删除的文件**。 

git add -u ：他仅监控**已经被add的文件**（即tracked file），他会将被修改的文件提交到暂存区。add  -u 不会提交新文件（untracked file）。（git add --update的缩写）

git add -A ：是上面两个功能的合集（git add --all的缩写）

### 查看

* **git status**               查看仓库状态（是否修改）
* **git diff readme.txt**       查看修改内容
* **git log**                  命令显示从最近到最远的提交日志 加参数--pretty=oneline简化

```
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

 类似`1094adb...`的是`commit id`（版本号） 

## 版本回退

**$ git reset --hard HEAD^    版本回退**
**$ git reset --hard 版本号**
**$ git reflog                用来记录你的每一次命令**



HEAD^为回退到上一个版本，HEAD^^回退到上上个版本



> 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
>
> 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
>
> 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

**$ git checkout               其实是用版本库里的版本替换工作区的版本**

## 删除文件

rm 文件

* **git rm**                     用于从版本库删除一个文件，需要commit

* **git checkout -- test.txt**  **从版本库恢复文件到工作区**





## 使用远程仓库

1.  **创建SSH Key**。是否有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`（私钥）和`id_rsa.pub`（公钥）这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key，一路回车。

```ssh
$ ssh-keygen -t rsa -C "youremail@example.com"
```

2. github-->settiing-->SSH and GPG keys-->add keys,**添加公钥**

3. **关联**。 Create a new repo 新建仓库，关联

 ```
git remote add origin git@server-name:path/repo-name.git
 ```

   github关联

```
git remote add origin git@github.com:michaelliao/learngit.git
```

```
git push -u origin master    第一次推送master分支的所有内容；
git push origin master       推送最新修改；
```

4. **从远程仓库克隆**

```
 git clone git@github.com:michaelliao/gitskills.git克隆项目
```

## 分支

```
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>

创建+切换分支：git checkout -b <name>

合并某分支到当前分支：git merge <name>

删除分支：git branch -d <name>
强制删除没有被合并的分支 git branch -D <name>
git log --graph命令可以看到分支合并图

git stash   暂存工作区并清空修改（这样可以创建新分支）
git stash list 查看暂存的工作现场
git stash apply stash@{0} 恢复工作现场
git stash drop 删除工作现场
git stash pop 恢复并从stash中删除
```

## 多人工作模式

多人协作的工作模式通常是这样：

1. 首先，可以试图用git push origin <branch-name>推送自己的修改；

2. 如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；

3. 如果合并有冲突，则解决冲突，并在本地提交；

4. 没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！

   如果git pull提示no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream-to <branch-name> origin/<branch-name>。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

> **小结**
> 查看远程库信息，使用git remote -v；
>
> 本地新建的分支如果不推送到远程，对其他人就是不可见的；
>
> 从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
>
> 在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
>
> 建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
>
> 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

## 标签管理

1. 切换到需打标签的分支

```
git checkout master
git tag v1.0
```

2. 查看所有标签

```
$ git tag
```

3. 打历史标签，先查看历史提交commit id,

```
$ git log --pretty=oneline --abbrev-commit
$ git tag v0.9 f52c633
```

4. 查看标签信息

```
 $ git show v0.9
```

5. 推送标签到远程

```
$ git push origin v1.0
```

## 搭建git服务器

# IDEA 


###  为IDEA指定git路径 

 默认情况下，IDEA是不自带git运行程序的，所以需要通过
菜单->settings->Version Control->Git->Path to Git executable: 设置为[安装git](https://how2j.cn/k/idea/idea-git-install/1359.html)中所安装的cmd/git.exe 

 ![为IDEA指定git路径](https://stepimagewm.how2j.cn/5794.png) 

###  设置github账号 

接下来为github设置账号密码：
菜单->settings->Version Control->GitHub->Create API Token 

 ![设置github账号](https://stepimagewm.how2j.cn/5795.png) 

###  pull一个项目到本地 

 菜单->VCS->Chekout from Version Control->GitHub 

 ![checkout](https://stepimagewm.how2j.cn/5797.png) 

 ![输入项目参数](https://stepimagewm.how2j.cn/5798.png) 

### 创建项目

1.新建github仓库

 ![在Git上新建仓库](https://stepimagewm.how2j.cn/5642.png) 

 ![输入仓库名称](https://stepimagewm.how2j.cn/5643.png) 

创建成功，得到git地址

 ![创建成功，得到git地址](https://stepimagewm.how2j.cn/5644.png) 

2.建立本地仓库

 菜单->VCS->import into Version Control->Create Git Repository->e:\project\hiworld-OK 

 ![建立本地仓库](https://stepimagewm.how2j.cn/5805.png) 

3. 把项目加入到本地仓库

 右键项目->Git->Add 

 ![把项目加入到本地仓库](https://stepimagewm.how2j.cn/5806.png) 

4. 提交项目

 右键项目->Git->Commit Directory之后弹出如图所示的窗口，在Commit Message 输入 test， 然后点击 Commit And Push 

 ![提交项目](https://stepimagewm.how2j.cn/5807.png) 

 这里会询问你要提交的哪里去，点击Define remote,并输入在 [创建成功，得到git地址 ](https://how2j.cn/k/idea/idea-create/1364.html#step5804)步骤中的地址，然后点击push

 ![Push Commit](https://stepimagewm.how2j.cn/5808.png) 

5. 查看github



### 提交更新

*  使用快捷键**CTRL+K**,就会弹出提交的界面，点击Commit and Push即可 
*  使用命令行 git add .



# 其他问题

## 提交文件名字长，无法提交

```
git config --system core.longpaths true
```

## git如何push时不输入密码?

1. git 可以用 https 方式访问也可以用 ssh 方式访问，其中 https 就是你每次要输入密码那种了，ssh的话可以不用输入密码，但是安全哪里来呢 —— 就是密钥！ 密钥git 密钥使用 ssh-keygen 生成，分为 私钥和公钥，私钥本地保存，公钥放到服务端，github,osc git 等都差不多的设置。
2. https 和 ssh 的仓库地址不一样，如 开源中国的仓库 上提供了个按钮让你复制，htttps格式：[https://git.oschina.net/user_name/project_name.git](https://link.zhihu.com/?target=https%3A//git.oschina.net/chenjiancan/gamixyz_document.git)   git 格式： git@[git.oschina.net:user_name/project_name.git](https://link.zhihu.com/?target=https%3A//git.oschina.net/chenjiancan/gamixyz_document.git)

## 忽略特殊文件

 在Git工作区的根目录下创建一个特殊的`.gitignore`文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。 

GitHub已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore

忽略文件的原则是：

1. 忽略操作系统自动生成的文件，比如缩略图等；
2. 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件；
3. 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。

```
# Windows:
Thumbs.db
ehthumbs.db
Desktop.ini

# Python:
*.py[cod]
*.so
*.egg
*.egg-info
dist
build

# My configurations:
db.ini
deploy_key_rsa
```

# 参考

[廖雪峰的git教程]: https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664

