---
layout: post
title: '我的开发日常 Git 命令清单'
---


从 2011 年开始接触 Git 到现在，我的使用时间不算太短，但是却只限于 `git add`、`git commit`、`git push` 这几个简单命令。最近因为工作需要，我将 [Pro Git](https://git-scm.com/book/zh/v2) 通读了一遍，故写此篇。


## 一、前言

![Git 工作流](https://infp.github.io/blogimages/git-workflow.png){:.center}

Git 是一个分布式的版本控制系统，是指 Git 的远程仓库（Remote）和本地仓库（Repository）具有同等的地位，保存了代码的所有历史记录。上面的图来自阮一峰的[博客](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)，表示在 Git 的各个状态间相互切换的命令。

## 二、创建代码仓库

有两种方法来建立 Git 代码仓库。第一种是在现有目录下直接生成：

~~~sh
$ git init
~~~

另外一种是从服务器上直接克隆一个远程的仓库，比如：

~~~sh
$ git clone https://github.com/myanbin/jsterm.git
~~~

## 三、配置 Git

在初次运行 Git 前，需要配置一下用户信息和编辑器偏好：

~~~sh
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
~~~

配置你喜欢的编辑器，这样当 Git 需要输入信息时，便会调用它：

~~~sh
$ git config --global core.editor vim
~~~

Git 有三个级别的配置文件，分别为系统级的 `/etc/gitconfig`，用户级的 `~/.gitconfig` 和代码仓库级的 `.git/config`，每一个级别覆盖上一级别的配置。一般地，我们只需要读取用户级和代码仓库级的配置即可。

另外，我们可以通过设置别名，将较长的 Git 命令简写：

~~~sh
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
~~~

这样，当要输入 `git status` 时，只需要输入 `git st` 即可。


## 四、添加和提交

本地的代码仓库由 Git 维护的三颗 Tree 组成。第一个是工作目录（Workspace），它持有实际的文件；第二个是暂存区（Index/Stage），它像一个缓存区，临时保存即将要提交的改动；第三个是代码仓库（Repository），它有一个 HEAD 指针，用于指向你最后一次提交的结果。

![Git 的三个区域](https://infp.github.io/blogimages/git-areas.png){:.center}

当你再工作目录中修改了某个文件后，使用 `git add` 命令，可以将你在工作目录下的改动添加到暂存区，以便准备提交。比如：

~~~sh
$ echo hello,world > README.md
$ git add README.md
~~~

这个时候再查看 Git 仓库状态，应该类似于下面：

~~~sh
$ git status
On branch master
Your branch is up-to-date with 'origin/master'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md

~~~

如果要添加多个文件到暂存区，也可以使用 `git add -i` 进行交互式提交。

`git add` 的本质是维护一个准备提交的改动清单。所以执行 `git add` 时，添加到暂存区的是改动而不是文件。比如上面的例子，当用 `git add` 添加 `README.md` 之后，`git commit` 会将这次改动提交。但是假如没来得及 `git commit`，你又在 `README.md` 文件末尾添加了一行，如果你没有再次 `git add README.md`，这次修改是不会被提交的。

> 其实在 Git 中，每次运行 `git add <filename>` 时，便会计算该文件的 SHA-1 哈希值作为本次改动的唯一标识。