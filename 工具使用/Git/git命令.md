## …or create a new repository on the command line

但是我们主要会用的命令其实也就这么几个，没有这么花里胡哨的。

```shell
echo "# Notes" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/SidewinderAK47/Notes.git
git push -u origin master
```

## …or push an existing repository from the command line

```shell
git remote add origin https://github.com/SidewinderAK47/Notes.git
git push -u origin master
```

# 命令记录

## 初始化
### 初始化仓库
```shell
git init # 初始本地仓库 
```
### 签名
#### 本地级别签名
```shell
git config user.name [AAA]
git config user.email [邮箱地址]
#签名信息位置：cat .git/config
```
#### 系统级别签名
```shell
git config --globaluser.name [AAA]
git config --global user.email [邮箱地址]
# 签名信息位置：cd ~ 、cat .gitconfig
```
### 基本操作[查看状态+添加文件+提交操作]
a)、查看状态： git status(查看工作区、暂存区的状态)

b)、添加操作: git add 文件名(将工作区新建/修改的内容添加到暂存区)

c)、提交操作： git commit -m “commit message” 文件名(将暂存区的内容提交到本地库)

d)、提交远程仓库：git push origin master  提交到远程仓库某分支

### 历史操作记录查看
a)、git log
b)、git log --pretty=oneline
c)、git log --oneline
d)、git reflog (HEAD@{移动到当前版本需要多少步})

## 远程仓库的使用

### 查看远程仓库

：`git remote`

要查看当前配置有哪些远程仓库，可以用 `git remote` 命令，它会列出每个远程库的简短名字。在克隆完某个项目后，至少可以看到一个名为 origin 的远程库，Git 默认使用这个名字来标识你所克隆的原始仓库：

```
$ git clone git://github.com/schacon/ticgit.git
Cloning into 'ticgit'...
remote: Reusing existing pack: 1857, done.
remote: Total 1857 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (1857/1857), 374.35 KiB | 193.00 KiB/s, done.
Resolving deltas: 100% (772/772), done.
Checking connectivity... done.
$ cd ticgit
$ git remote
origin
```

也可以加上 `-v` 选项（译注：此为 `--verbose` 的简写，取首字母），显示对应的克隆地址：

```shell
$ git remote -v
origin  git://github.com/schacon/ticgit.git (fetch)
origin  git://github.com/schacon/ticgit.git (push)
```

如果有多个远程仓库，此命令将全部列出。比如在我的 Grit 项目中，可以看到：

```
$ cd grit
$ git remote -v
bakkdoor  git://github.com/bakkdoor/grit.git
cho45     git://github.com/cho45/grit.git
defunkt   git://github.com/defunkt/grit.git
koke      git://github.com/koke/grit.git
origin    git@github.com:mojombo/grit.git
```

### 添加远程仓库

要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用，运行 `git remote add [shortname] [url]`：

```
$ git remote
origin
$ git remote add pb git://github.com/paulboone/ticgit.git
$ git remote -v
origin  git://github.com/schacon/ticgit.git
pb  git://github.com/paulboone/ticgit.git
```

### 推送数据到远程仓库

项目进行到一个阶段，要同别人分享目前的成果，可以将本地仓库中的数据推送到远程仓库。实现这个任务的命令很简单： `git push [remote-name] [branch-name]`。如果要把本地的 master 分支推送到 `origin` 服务器上（再次说明下，克隆操作会自动使用默认的 master 和 origin 名字），可以运行下面的命令：

```
$ git push origin master
$ git push [remote-name] [branch-name]
```

只有在所克隆的服务器上有写权限，或者同一时刻没有其他人在推数据，这条命令才会如期完成任务。如果在你推数据前，已经有其他人推送了若干更新，那你的推送操作就会被驳回。你必须先把他们的更新抓取到本地，合并到自己的项目中，然后才可以再次推送。有关推送数据到远程仓库的详细内容见第三章。