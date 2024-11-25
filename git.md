[TOC]

# 一、Git

## 1.集中式VS分布式

- **集中式版本控制系统**：

  版本库是集中存放在中央服务器的。中央服务器就好比是一个图书馆，你要改一本书，必须先从图书馆借出来，然后回到家自己改，改完了，再放回图书馆。

  弊端：必须联网才能工作

- **分布式版本控制系统**：

  每个人的电脑上都是一个完整的版本库，这样，你工作的时候，就不需要联网了，因为版本库就在你自己的电脑上。安全性更高。



## 2.Git介绍

流行的分布式版本控制系统



# 二、本地操作



## 1.安装

windows下安装完

```
git config --global user.name "Your Name"
```



```
git config --global user.email "email@example.com"
```



## 2.创建版本库

**什么是版本库？**

版本库又名仓库（Repository），你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。



创建一个版本库非常简单

### 创建文件夹

①windows环境：首先，选择一个合适的地方，创建一个空目录，在目录下右键打开git bash

②Linux环境：

```
mkdir -p banben
cd banben
pwd
```

### 初始化指令：git init

通过`git init`命令把这个目录变成Git可以管理的仓库：

```
git init
```

### 提交命令

①编写一个`readme.txt`文件，内容如下：

```
Git is a version control system.
Git is free software.
```

②用命令`git add`告诉Git，把文件添加到仓库：

```
git add readme.txt
```

③用命令`git commit`告诉Git，把文件提交到仓库：

```
git commit -m "write a readme file"
```

备注：

git add命令用于将工作目录中的修改添加到暂存区（Stage）

git commit命令用于将暂存区中的内容提交到Git仓库中



## 3.时光机穿梭

### 序言

①继续修改readme.txt文件，改成如下内容：

```
Git is a distributed version control system.
Git is free software.
```

运行`git status`命令看看结果：

```
git status
```

多次修改后记不得之前怎么修改的，可以使用`git diff`这个命令看看：

```
git diff readme.txt
```

继续执行`git add`：

```
git add readme.txt
```

在执行第二步`git commit`之前，我们再运行`git status`看看当前仓库的状态：

```
git status
```

看到修改信息的文件，就可以从暂存区提交到仓库并备注了

```
git commit -m "add distributed"
```

提交后，我们再用`git status`命令看看仓库的当前状态：

```
git status
```

接下来第三次修改提交一下

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
```

```
git add readme.txt
```

```
git commit -m "append GPL"
```

### 版本回退

我们用`git log`命令查看历史记录：

```
git log
```

信息太多可以简化

```
git log --pretty=oneline
```

在Git中，用`HEAD`表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`



回头到上一个版本命令：

```
git reset --hard HEAD^
```

`--hard`参数有啥意义？

`--hard`会回退到上个版本的已提交状态，

`--soft`会回退到上个版本的未提交状态，

`--mixed`会回退到上个版本已添加但未提交的状态。



后悔药：

`git reflog`用来记录你的每一次命令：找到那个需要的`commit id`，进行返回

```
git reset --hard 1094a     #1094a是查到的commit id
```



### 工作区 & 暂存区

#### 工作区

工作区（Working Directory）就是你在电脑里能看到的目录，比如我的`banben`文件夹就是一个工作区

#### 版本库

版本库（Repository）：工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库.



Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

![](D:\workspaces\git_test\src\ku.png)

从来没有被添加过，状态是`Untracked`



### 管理修改

如果当你修改第一次git add,继续修改文件，修改完没用git add，而用了git commit

实际是递交了第一次暂存区的修改到库里。

`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别

```
git diff HEAD -- readme.txt
```



### 撤销修改

命令`git checkout -- readme.txt`意思就是，把`readme.txt`文件在工作区的修改全部撤销。会有两种情况出现：

①一种是`readme.txt`自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

②一种是`readme.txt`已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

总之，就是让这个文件回到最近一次`git commit`或`git add`时的状态。



在`commit`之前，你发现了这个问题。用`git status`查看一下，修改只是添加到了暂存区，还没有提交。这时可以用命令`git reset HEAD <file>`可以把暂存区的修改撤销掉（unstage），重新放回工作区：

`git reset`命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用`HEAD`时，表示最新的版本。

丢弃工作区的修改

```plain
git checkout -- readme.txt
```



### 删除文件

删除也是一个修改操作，我们实战一下，先添加一个新文件`test.txt`到Git并且提交：

```plain
git add test.txt
```

```plain
git commit -m "add test.txt"
```

一般情况下，你通常直接在文件管理器中把没用的文件删了，或者用`rm`命令删了：

```plain
rm test.txt
```

这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

```plain
git status
```

现在你有两个选择，

一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```plain
git rm test.txt
```

```plain
git commit -m "remove test.txt"
```

现在，文件就从版本库中被删除了。



另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

```plain
git checkout -- test.txt
```

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。



# 三、远程仓库

如果只是在一个仓库里管理文件历史，Git和SVN真没啥区别

Git是分布式版本控制系统，同一个Git仓库，可以分布到不同的机器上

最早，肯定只有一台机器有一个**原始版本库**，此后，别的机器可以“**克隆**”这个原始版本库，而且每台机器的版本库其实**都是一样的**，并没有主次之分。



实际情况往往是这样，找一台电脑充当**服务器**的角色，每天24小时开机，其他每个人都从这个“服务器”仓库克隆一份到自己的电脑上，并且各自把各自的**提交推送**到服务器仓库里，也从服务器仓库中**拉取**别人的提交。



### github

①创建SSH Key

在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。

如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```
ssh-keygen -t rsa -C "youremail@example.com"
```

一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码

`id_rsa`和`id_rsa.pub`这两个文件在：

c盘：/user(或者是用户)/你的用户名(你自己之前起过的)/.ssh



`id_rsa`是私钥，不能泄露出去，

`id_rsa.pub`是公钥，可以放心地告诉任何人



②登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

![](D:\workspaces\git_test\src\add-key.png)

“Add Key”，你就应该看到已经添加的Key：

![](D:\workspaces\git_test\src\keys.png)

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的

当然，GitHub允许你添加多个Key。只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。



## 1.添加远程库

本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步

①首先，登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：

![](D:\workspaces\git_test\src\new-repo.png)

②在Repository name填入`learngit`，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：

![](D:\workspaces\git_test\src\create-repo.png)

在GitHub上的这个`learngit`仓库还是空的，GitHub告诉我们：可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。



根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```plain
git remote add origin git@github.com:your_github_name/learngit.git
```

添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库



注意：很可能失败，由于22端口可能被防火墙屏蔽了，可以尝试连接GitHub的443端口：

```
vim ~/.ssh/config
```

```
# Add section below to it
Host github.com
  Hostname ssh.github.com
  Port 443
```

:wq保存，再测试一下能否连通

```bash
ssh -T git@github.com
```



下一步，就可以把本地库的所有内容推送到远程库上：

```plain
git push -u origin master
```

第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。



推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：

从现在起，只要本地作了提交，就可以通过命令：

```plain
git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！



## 2.删除远程库

如果添加的时候地址写错了，或者就是想删除远程库，可以用`git remote rm <name>`命令。

使用前，建议先用`git remote -v`查看远程库信息：

```plain
git remote -v
```

然后，根据名字删除，比如删除`origin`：

```plain
 git remote rm origin
```

此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。

远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，在后台页面找到删除按钮再删除。



## 3.克隆远程库

从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆

①首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

![](D:\workspaces\git_test\src\create-repo.png)

勾选`Initialize this repository with a README`，GitHub会自动为我们创建一个`README.md`文件。

下一步是用命令`git clone`克隆一个本地库：

```plain
git clone git@github.com:primeyoung/gitskills.git
```

然后进入`gitskills`目录看看，已经有`README.md`文件了：





# 四、分支管理

Git的分支无论创建、切换和删除分支，Git在1秒钟之内就能完成！无论你的版本库是1个文件还是1万个文件。

## 1.创建与合并分支

每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。



①一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![](D:\workspaces\git_test\src\head01.png)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。



②当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![](D:\workspaces\git_test\src\fenzhi.png)

Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！



③不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![](D:\workspaces\git_test\src\fenzhi02.png)

④假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![](D:\workspaces\git_test\src\fenzhi03.png)



⑤合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![](D:\workspaces\git_test\src\fenzhi04.png)



哪些提交是分支完成的，实战：

①我们创建`dev`分支，然后切换到`dev`分支：

```plain
git checkout -b dev
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```plain
git branch dev
git checkout dev
```



②然后，用`git branch`命令查看当前分支：

```plain
git branch
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：Creating a new branch is quick.

然后提交：

```plain
git add readme.txt
git commit -m "branch test"
```

③现在，`dev`分支的工作完成，我们就可以切换回`master`分支：

```plain
git checkout master
```

切换回`master`分支后，再查看一个`readme.txt`文件，刚才添加的内容不见了！因为那个提交是在`dev`分支上，而`master`分支此刻的提交点并没有变：

![](D:\workspaces\git_test\src\fenzhi05.png)

④现在，我们把`dev`分支的工作成果合并到`master`分支上：

```plain
git merge dev
```

`git merge`命令用于合并指定分支到当前分支。

合并后，再查看`readme.txt`的内容，就可以看到，和`dev`分支的最新提交是完全一样的。

命令输出可以看到`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快，当然也不是每次合并都能`Fast-forward`。



⑤合并完成后，就可以放心地删除`dev`分支了：

```plain
git branch -d dev
```

删除后，查看`branch`，就只剩下`master`分支了：

```plain
git branch
```



备注：

切换分支使用`git checkout <branch>`，而撤销修改是`git checkout -- <file>`，同一个命令

最新版本的Git提供了新的`git switch`命令来切换分支：

①创建并切换到新的`dev`分支，可以使用：

```plain
git switch -c dev
```

②直接切换到已有的`master`分支，可以使用：

```plain
git switch master
```





## 2.解决冲突

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。



案例：

### ①新建分支操作

```plain
git switch -c feature1
```

修改`readme.txt`最后一行，改为：

```plain
Creating a new branch is quick AND simple.
```

在`feature1`分支上提交：

```plain
git add readme.txt
```

```plain
git commit -m "AND simple"
```

### ②master分支操作

```plain
git switch master
```

Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交

在`master`分支上把`readme.txt`文件的最后一行改为：

```plain
Creating a new branch is quick & simple.
```

提交：

```plain
git add readme.txt 
```

```plain
git commit -m "& simple"
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![](D:\workspaces\git_test\src\chongtu01.png)

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突

```plain
git merge feature1
```

Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

```plain
git status
```

Git用`<<<<<<<`，`=======`，`>>>>>>>`标记出不同分支的内容，我们修改如下后保存：

```plain
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick and simple.
```

再提交：

```plain
git add readme.txt 
```

```plain
git commit -m "conflict fixed"
```

现在，`master`分支和`feature1`分支变成了下图所示：

![](D:\workspaces\git_test\src\chongtu02.png)

用带参数的`git log`也可以看到分支的合并情况：

```plain
git log --graph --pretty=oneline --abbrev-commit
```

最后，删除`feature1`分支：

```plain
git branch -d feature1
```

## 3.分支管理策略

合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，从分支历史上就可以看出分支信息。

实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```plain
git switch -c dev
```

修改readme.txt文件，并提交一个新的commit：

```plain
git add readme.txt 
```

```plain
git commit -m "add merge"
```

现在，我们切换回`master`：

```plain
git switch master
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```plain
git merge --no-ff -m "merge with no-ff" dev
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```plain
git log --graph --pretty=oneline --abbrev-commit
```

可以看到，不使用`Fast forward`模式，merge后就像这样：

![](D:\workspaces\git_test\src\fenzhi_guanli.png)

### 分支策略

1.首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

2.干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

![](D:\workspaces\git_test\src\fenzhi_celue.png)



## 4.BUG分支



