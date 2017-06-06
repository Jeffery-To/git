![git](./git.jpg)

# git
[git-for-windows](https://git-for-windows.github.io/)

## git教程

- [简单易学的Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
- [Git 实战教程](https://www.shiyanlou.com/courses/4)

## git命令

### git config：配置相关信息

使用Git的第一件事就是设置你的名字和email，这些就是你在提交`commit`时的签名，每次提交记录里都会包含这些信息。使用`git config`命令进行配置：
```
$ git config --global user.name "Scott Chacon"
$ git config --global user.email "schacon@gmail.com"
```
执行了上面的命令后，会在当前用户目录下建立一个叫.gitconfig 的文件。

如果你想使项目里的某个值与前面的全局设置有区别(例如把私人邮箱地址改为工作邮箱)，你可以在项目中使用`git config` 命令不带 `--global` 选项来设置。 这会在你当前的项目目录下创建 `.git/config`，从而使用针对当前项目的配置。

### git clone：复制仓库

为了得到一个项目的拷贝(copy)，我们需要知道这个项目仓库的地址(Git URL)。Git能在许多协议下使用，所以Git URL可能以ssh://，http(s)://，git://。有些仓库可以通过不只一种协议来访问。
`$ git clone http://git.xxx.com/xxx/xxx`

### git init：初始化仓库

可以对一个已存在的文件夹用下面的命令让它置于Git的版本控制管理之下。
```
创建代码目录project：
$ cd /home/xxx/
$ mkdir project
$ cd project
$ git init
Git会输出:
Initialized empty Git repository in /home/shiyanlou/project/.git/
```

### git add：添加更新内容到索引中

使用`git add`命令添加新创建或修改的文件到**本地的缓存区（Index）**

### git diff：比较内容

- 比较提交

`git diff`会显示当前你所有已做的但没有加入到索引里的修改。`git diff`命令输出当前工作目录中修改的内容，并不包含新加文件，请注意这些内容还没有添加到本地缓存区。
`git diff --cached`查看缓存区内与上次提交之间的差别，输出中已经包含了新加文件的内容。
`git commit`提交后git diff与git diff --cached都不会有任何输出了。

- 比较分支

查看test分支和master之间的差别：`$ git diff master test`

- 更多的比较选项

`git diff` 是一个难以置信的有用的工具，可以找出你项目上任意两个提交点间的差异。可以使用git help diff详细查看其他参数和功能。

如果你要查看当前的工作目录与另外一个分支的差别，你可以用下面的命令执行:
```
# 切换到master
$ git checkout master
# 查看与test分支的区别
$ git diff test
```
你也以加上路径限定符，来只比较某一个文件或目录：`$ git diff test file1`。
`--stat` 参数可以统计一下有哪些文件被改动，有多少行被改动：`$ git diff test --stat`

### git status：获取当前项目状况

要随时掌握工作区的状态，使用`git status`命令。
如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

### git commit：提交

使用`git commit`命令提交到本地代码库

当所有新建，修改的文件都被添加到了缓存区，我们就要使用`git commit`提交到本地仓库：`$ git commit -m "注释"`。
除了用`git add` 命令，我们还可以用下面的命令将所有没有加到缓存区的修改也一起提交，但`-a`命令不会添加**新建的文件**：
`$ git commit -a -m "注释"`。
需要注意的是如果是修改文件，也需要使用`git add`命令添加到缓存区才可以提交。如果是删除文件，则直接使用`git rm`命令删除后会**自动**将已删除文件的信息添加到缓存区，`git commit`提交后就会将本地仓库中的对应文件删除。

当用命令git rm删掉，并且git commit后，文件就从版本库中被删除了。
当用命令git rm删掉删错了，但版本库里还有，所以可以用`$ git checkout -- test.txt`命令很轻松地把误删的文件恢复到最新版本。
`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是**修改**还是**删除**，都可以“一键还原”。

### git reset：恢复版本
### git reflog：查看命令历史

如果我们准备把项目回退到上一个版本。
在Git中，用HEAD表示当前版本，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个^比较容易数不过来，所以写成`HEAD~100`。
可以使用`git reset --hard commit_id`命令：
```
$ git reset --hard HEAD^
HEAD is now at ea34578 add distributed
```
你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？Git提供了一个命令`git reflog`用来记录你的每一次命令：
```
$ git reflog
ea34578 HEAD@{0}: reset: moving to HEAD^
3628164 HEAD@{1}: commit: append GPL
ea34578 HEAD@{2}: commit: add distributed
cb926e7 HEAD@{3}: commit (initial): wrote a readme file
```
现在可以指定回到某个版本：
```
$ git reset --hard 3628164
HEAD is now at 3628164 append GPL
```
用`git log`可以查看提交历史，以便确定要回退到哪个版本。
用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

### git branch：分支相关

Git的分支可以让你在主线（master分支）之外进行代码提交，同时又不会影响代码库主线。
创建分支：`$ git branch xxx`
运行`git branch`命令可以查看当前的分支列表，已经目前的开发环境处在哪个分支上：
```
$ git branch
 xxx
* master
```

### git checkout：切换分支

输入`git checkout 分支名`可以切换到其他分支.
```
$ git checkout xxx
Switched to branch 'xxx'
```
创建+切换分支：`git checkout -b <name>`

### git merge：合并分支

可以通过下面的`git merge`命令来合并experimental到主线分支master:
```
# 切换到master分支
$ git checkout master
# 将experimental分支合并到master
$ git merge  -m 'merge experimental branch' experimental
-m参数仍然是需要填写合并的注释信息。
```
如果有冲突，比如两个分支都改了一个文件file3，则合并时会失败。
```
$ git checkout master
$ git merge experimental
Auto-merging file3
CONFLICT (content): Merge conflict in file3
Automatic merge failed; fix conflicts and then commit the result.
```
合并失败后先用`git status`查看状态，会发现file3显示为both modified，查看file3内容会发现：
```
$ cat file3
test
<<<<<<< HEAD
master: update file3
=======
experimental: update file3
>>>>>>> experimental
```
可以看到冲突的内容都被添加到了file3中，我们使用vim编辑这个文件，去掉git自动产生标志冲突的<<<<<<等符号后，根据需要只保留我们需要的内容后保存，然后使用`git add file3`和`git commit`命令来提交合并后的file3内容，**这个过程是手动解决冲突的流程**。

当我们完成合并后，不再需要experimental时，可以使用下面的命令删除：
`$ git branch -d experimental`
`git branch -d`只能删除那些已经被当前分支的合并的分支。如果你要强制删除某个分支的话就用`git branch –D`删除未合并分支。

当合并后感觉很乱，可以撒销合并`$ git reset --hard HEAD^`

**合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。**

**分支策略**
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
所以，团队合作的分支看起来就像这样：
![分支策略](./git-branch.png)

### git log：查看日志

`git log`命令可以显示所有的提交(commit)：

- `git log`有很多选项，可以使用`git help log`查看，例如下面的命令就是找出所有从"v2.5“开始在fs目录下的所有Makefile的修改：
`$ git log v2.5.. Makefile fs/`

- 如果用`--stat`选项使用'git log',它会显示在每个提交(commit)中哪些文件被修改了, 这些文件分别添加或删除了多少行内容，这个命令相当于打印详细的提交记录：`$ git log --stat`

- 你可以按你的要求来格式化日志输出。`--pretty` 参数可以使用若干表现格式，如`oneline`:`$ git log --pretty=oneline`
或者你也可以使用 short 格式: `$ git log --pretty=short`
你也可用`medium`,`full`,`fuller`,`email` 或`raw`。 如果这些格式不完全符合你的相求， 你也可以用`--pretty=format`参数定义格式。
`--graph` 选项可以可视化你的提交图(commit graph)，会用ASCII字符来画出一个很漂亮的提交历史(commit history)线：
`$ git log --graph --pretty=oneline`

- 日志记录可以按不同的顺序来显示。如果你要指定一个特定的顺序，可以为`git log`命令添加顺序参数。
按默认情况，提交会按逆时间顺序显示，可以指定`--topo-order`参数，让提交按拓扑顺序来显示(就是子提交在它们的父提交前显示):`$ git log --pretty=format:'%h : %s' --topo-order --graph`你也可以用 `--reverse`参数来逆向显示所有提交日志。

### git remote： 查看远程库的信息

当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。

要查看远程库的信息，用`git remote`：
```
$ git remote
origin
```
或者，用git remote -v显示更详细的信息：
```
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```
上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。

### git push: 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
`$ git push origin master`
如果要推送其他分支，比如dev，就改成：
`$ git push origin dev`

### git pull: 抓取分支

`git pull`命令执行两个操作: 它从远程分支(remote branch)抓取修改`git fetch`的内容，然后把它合并`git merge`进当前的分支。

多人协作时，大家都会往master和dev分支上推送各自的修改。
当从远程库clone时，默认情况下，只能看到本地的master分支。
要在dev分支上开发，就必须创建远程origin的dev分支到本地，用这个命令创建本地dev分支：
`$ git checkout -b dev origin/dev`
现在，就可以在dev上继续修改，然后，时不时地把dev分支push到远程：
`$ git push origin dev`

多人协作的工作模式通常是这样：
首先，可以试图用`git push origin branch-name`推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
如果`git pull`提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`。
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用`git push origin branch-name`推送就能成功！

### git tag: 标签相关

命令`git tag <name>`就可以打一个新标签：
`$ git tag v1.0`
默认标签是打在最新提交的commit上的。

要对注释add merge这次提交打标签，它对应的commit id是6224937，敲入命令：
`$ git tag v0.9 6224937`
再用命令git tag查看标签：
```
$ git tag
v0.9
v1.0
```
可以用`git show <tagname>`查看标签信息：
```
$ git show v0.9
commit 622493706ab447b6bb37e4e2a2f276a20fed2ab4
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Thu Aug 22 11:22:08 2013 +0800

    add merge
...
```
还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：
`$ git tag -a v0.1 -m "version 0.1 released" 3628164`

如果标签打错了，也可以删除：
```
$ git tag -d v0.1
Deleted tag 'v0.1' (was e078af9)
```
因为创建的标签都只存储在本地，不会自动推送到远程。
如果要推送某个标签到远程，使用命令git push origin <tagname>：
`$ git push origin v1.0`
或者，一次性推送全部尚未推送到远程的本地标签：
`$ git push origin --tags`
如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
`$ git tag -d v0.9`
然后，从远程删除。删除命令也是push，但是格式如下：
`$ git push origin :refs/tags/v0.9`
