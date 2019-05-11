# 读GitHub入门与实践

标签（空格分隔）： 工具类书籍笔记

---

### 1.GitHub提供的主要功能

#### Git仓库
#### Issue
Issue功能，是将一个任务或问题分配给一个Issue进行追踪和管理的功能。可以像 BUG 管理系统或 TiDD（Ticket-driven Development）的 Ticket 一样使用。在 GitHub上，每当进行我们即将讲解的PullRequest，都会同时创建一个 Issue。每一个功能更改或修正都对应一个Issue，讨论或修正都以这个 Issue 为中心进行。只要查看Issue，就能知道和这个更改相关的一切信息，并以此进行管理。在Git的提交信息中写上Issue的ID（例如“#7”），GitHub 就会自动生成从Issue到对应提交的链接。另外，只要按照特定的格式描述提交信息，还可以关闭 Issue。

#### Wiki
通过 Wiki 功能，任何人都能随时对一篇文章进行更改并保存，因此可以多人共同完成一篇文章。该功能常用在开发文档或手册的编写中。

### 2.Git的导入

#### 2.1什么是版本管理
版本管理就是管理更新的历史记录。它为我们提供了一些在软件开发过程中必不可少的功能，例如记录一款软件添加或更改源代码的过程，回滚到特定阶段，恢复误删除的文件等。

#####集中型与分散型
集中型将所有数据集中存放在服务器当中，有便于管理的优点。但是一旦开发者所处的环境不能连接服务器，就无法获取最新的源代码，开发也就几乎无法进行。服务器宕机时也是同样的道理，而且万一服务器故障导致数据消失，恐怕开发者就再也见不到最新的源代码了。

分散型拥有多个仓库，相对而言稍显复杂。不过，由于本地的开发环境中就有仓库，所以开发者不必连接远程仓库就可以进行开发。

#### 2.4 初始设置
下面是对本地安装的Git进行设置。
**设置姓名和邮箱**
首先来设置使用Git时的姓名和邮箱地址。名字请用英文输入。

    $ git config --global user.name "Firstname Lastname"
    $ git config --global user.email "your_email@example.com"
    
这个命令，会在“~/.gitconfig”中以如下形式输出设置文件。
[user]
  name = Firstname Lastname
  email = your_email@example.com    
####提高命令输出的可读性
顺便一提，将 color.ui 设置为 auto可以让命令的输出拥有更高的可读性。
$ git config --global color.ui auto

### 3 使用GitHub的前期准备
####3.1 使用前的准备
创建账户
首先在github上创建账户。
创建仓库
连接仓库
**公开代码**
> * 提交

    $ git add hello_world.txt
$ git commit -m "Add hello_world script by php"
通过 git add 命令将文件加入暂存区 7 ，再通过 git commit 命令提交.

添加成功后，可以通过 git log 命令查看提交日志。
    $ git log
    
之后就进行push ,GitHub 上的仓库就会被更新。
    $git push
    这样一来代码就在GitHub上公开了。
    
### 4 通过实际操作学习Git
#### 4.1 基本操作
**Git init————初始化仓库**
要使用 Git 进行版本管理，必须先初始化仓库。Git 是使用 git init 命令进行初始化的。请实际建立一个目录并初始化仓库。

    $mkdir git-tutorial
$cd git-tutorial
    $git init
    如果初始化成功，执行了 git init 命令的目录下就会生成 .git 目录。这个 .git 目录里存储着管理当前目录内容所需的仓库数据。
    
**git status ————查看仓库状态**
git status 命令用于显示 Git 仓库的状态。这是一个十分常用的命令，请务必牢记。
工作树和仓库在被操作的过程中，状态会不断发生变化。在 Git 操作过程中时常用 git status 命令查看当前状态，可谓基本中的基本。下面，就让我们来实际查看一下当前状态。
    $git status
    # On branch master
    #
    # Initial commit
    #
    nothing to commit (create/copy files and use "git add" to track)
    
**git add————向暂存区中添加文件**
如果只是用 Git 仓库的工作树创建了文件，那么该文件并不会被记入 Git 仓库的版本管理对象当中。因此我们用 git status 命令查看 README.md 文件时，它会显示在 Untracked files 里。
要想让文件成为 Git 仓库的管理对象，就需要用 git add 命令将其加入暂存区（Stage 或者 Index）中。暂存区是提交之前的一个临时区域。

    $git add README.md
$ git status
    # On branch master
    #
    # Initial commit
    #
    # Changes to be committed:
    #   (use "git rm --cached <file>..." to unstage)
    #
    #       new file:   README.md
    将 README.md 文件加入暂存区后，git status 命令的显示结果发生了变化。可以看到，README.md 文件显示在 Changes to be committed 中了
    
**git commit ————提交 保存仓库的历史记录**
git commit 命令可以将当前暂存区中的文件实际保存到仓库的历史记录中。通过这些记录，我们就可以在工作树中复原文件。
……记述一行提交信息
我们来实际运行一下 git commit 命令。

    $ git commit -m "First commit"
[master (root-commit) 9f129ba] First commit
  1 file changed, 0 insertions(+), 0 deletions(-)
  create mode 100644 README.md

-m 参数后的 "First commit" 称作提交信息，是对这个提交的概述。

**git log——查看提交日志**
git log 命令可以查看以往仓库中提交的日志。包括可以查看什么人在什么时候进行了提交或合并，以及操作前后有怎样的差别。关于合并我们会在后面解说。
Author 栏中显示我们给 Git 设置的用户名和邮箱地址。Date 栏中显示提交执行的日期和时间。再往下就是该提交的提交信息。
如果只想让程序显示第一行简述信息，可以在 git log 命令后加上 --pretty=short 。这样一来开发人员就能够更轻松地把握多个提交。
$ git log --pretty=short
        　
commit 9f129bae19b2c82fb4e98cde5890e52a6c546922
Author: hirocaster <hohtsuka@gmail.com>
        　
    First commit

>* 只显示指定目录、文件的日志
只要在 git log 命令后加上目录名，便会只显示该目录下的日志。如果加的是文件名，就会只显示与该文件相关的日志。

$ git log README.md

>* 显示文件的改动
如果想查看提交所带来的改动，可以加上 - p 参数，文件的前后差别就会显示在提交信息之后。

$ git log -p

比如，执行下面的命令，就可以只查看 README.md 文件的提交日志以及提交前后的差别。

$ git log -p README.md


    






    















    




