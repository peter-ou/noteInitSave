## 第一次使用Git上传本地项目到github上的实现

<!-- GFM-TOC -->

- [实现的总体思路](#实现的总体思路)
- [git和github的相关联具体操作](#git和github的相关联具体操作)
- [后续git提交内容到GitHub仓库中操作](#后续git提交内容到github仓库中操作)
- [使用常用命令（表格命令前要加git）](#使用常用命令表格命令前要加git)
- [git学习参考教程](#git学习参考教程)
- [工作实践中有用到的](#工作实践中有用到的)
  - [1，git拉取远程某分支强制覆盖本地代码分支（与git远程仓库保持一致,例如master分支）](#1git拉取远程某分支强制覆盖本地代码分支与git远程仓库保持一致例如master分支)
  - [2，不同分支的合并命令;(例如我们要实现master 合并到 develop)](#2不同分支的合并命令例如我们要实现master-合并到-develop)

# 实现的总体思路

```
1、在本地创建一个版本库（即文件夹），通过git init把它变成Git仓库；

2、把项目复制到这个文件夹里面，再通过git add .把项目添加到仓库；

3、再通过git commit -m "注释内容"把项目提交到仓库；

4、在Github上设置好SSH密钥后，新建一个远程仓库，通过git remote add origin https://github.com/guyibang/TEST2.git将本地仓库和远程仓库进行关联；

5、最后通过git push -u origin master把本地仓库的项目推送到远程仓库（也就是Github）上；（若新建远程仓库的时候自动创建了README文件会报错，解决办法看上面）。

```

# git和github的相关联具体操作

1，下载Git软件：<https://git-scm.com/downloads，据说ios自带的有git软件，这个我就不太清楚了。>

2，下载之后安装就很简单了，一路下一步就可以了。安装完成后鼠标右击和者开始->程序会出现（如下图），打开Git Bash，进入bash界面。
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-12-50-58.png width='80%'/></div><br/>

3，执行git init 建好git 的本地仓库（可以有多个）,就是在本地创建一个版本库（即文件夹），通过git init把它变成一个Git仓库；

```shell
git init
```

假如我们建好的本地git仓库是： /d/studyNote/noteInit (master)

4，配置本地git信息

```shell
//说明：双引号中需要你的用户名，这个可以随便输入，比如“zhangsan”
git config --global user.name "user.name"  ##回车
//双引号中需要输入你的有效邮箱，比如“12131312@qq.com”
git config --global user.email "yourmail@youremail.com.cn"  ##回车
// 查看配置结果 
git config --list  ##回车  
```

5,查看密钥ssh keys 是否存在? 不存在则生成，存在则找到其位置。

```
cd ~/.ssh
```

若出现“No such file or directory”,则表示需要创建一个ssh keys。执行命令
`
ssh-keygen -t rsa -C yourmail@youremail.com.cn"
`
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-14-03-36.png width='80%'/></div><br/>

如果没有出现，就执行命名ssh-keygen，找到之前生成好的密钥的位置

<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-13-56-08.png width='80%'/></div><br/>

6，找到生成或存在的密钥文件,找到指定的目录打开公钥文件id_rsa.pub复制所有内容。
>/c/Users/oushuncai/.ssh/id_rsa
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-14-10-19.png width='80%'/></div><br/>

7, 登录自己的GitHub账号，找到右上角头像中的Settings。
进入Settings后,点击SSH and GPG keys,然后再点击右上角添加新密钥按钮New SSH key。
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-14-22-55.png width='80%'/></div><br/>

8，然后，将idb_rsa.pub里的内容拷贝到Key内，Title内容随便填，确定即可。
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-14-25-02.png width='80%'/></div><br/>

9，在Github上创建一个Git空仓库。
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-14-46-42.png width='80%'/></div><br/>

复制这个空仓库地址链接。

`
git@github.com:peter-ou/noteInitSave.git
`
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-14-50-45.png width='80%'/></div><br/>

10, 把要上传的到GitHub的文件夹放入 本地git仓库：例如, D:\studyNote\noteInit 中

11，git status来查看你当前的状态，如果文件内有东西会出现红色的字，不是绿色，这是正常的。
>查看代码或文件的状态（所处哪个区域）

```
git status
红色：工作区，还未提交到暂存区。
绿色：暂存区，还未提交到历史区。
若默认色，三个区域代码已经同步。
```

12，然后通过git add把项目添加到仓库（或git add .把该目录下的所有文件添加到仓库，注意点是用空格隔开的）。
>工作区提交到暂存区

```
git add xxx  //指定文件提交到暂存区
git add .   //全部提交到暂存区，包含修改和增加的，但不包含删除的
git add -u  //全部提交到暂存区，包含修改和删除的，但不包含新增的
git add -A  //  . 并且 -u
```

<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-15-17-14.png width='80%'/></div><br/>

13, 用git commit -m "注释" 把项目提交到本地Git仓库。

```
暂存区提交到历史区
git commit ：提交到历史区（此提交方式注意：需要备注操作信息）
git commit -m 'xxx' （操作描述）：提交到历史区
git log ： 查看提交记录
git reflog ：查看所有历史记录
```

<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-15-18-43.png width='80%'/></div><br/>

14, 现在远程gitGitHub仓库 和 本地git仓库及内容都已经准备好了。接下来我们进行本地git仓库和GitHub仓库的关联，实现将本地git仓库内容托管到GitHub上

```
1, git remote add origin git@github.com:peter-ou/noteInitSave.git(第9点复制的GitHub仓库地址)  // 添加远程仓库地址
2, git push -u origin master //关联好之后我们就可以把本地库的所有内容推送到远程仓库（也就是Github）上了. 
3, git push origin master //由于新建的远程仓库是空的，所以要加上-u 这个参数，等远程仓库里面有了内容之后，下次再从本地库上传内容的时候只需这样就可以了.
 
```

<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-15-42-21.png width='80%'/></div><br/>

15, 去GitHub上看我们提交的内容如下：
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-15-45-30.png width='80%'/></div><br/>

如上我们已经成功关联本地Git仓库和GitHub仓库，并托管GitHub成功。

# 后续git提交内容到GitHub仓库中操作

- **命令如下（重要，常用）：**

```
//add只是将文件或目录添加到了index暂存区
//使用commit可以实现将暂存区的文件提交到git本地仓库，[message]为备注信息
// 将本地git仓库内容提交到关联好了的 GitHub 仓库 
git add .
git commit -m "message"
git push origin master  // origin 为远程创库别名，也可以是其他名字，例如为GitHub


// 自定义远程仓库名称
1， git remote -v 命令可以查看远程仓库名称

2，删除GIt默认远程库名称
//git默认远程库名称的别名为origin
$ git remote rm origin

3，分别关联Gitee和GitHub并设置名称
//关联gitee并设置别名为gitee
$ git remote add gitee @git/gitee.com:admin/demo.git

//关联github并设置别名为github
$ git remote add github @git/github.com:admin/demo.git

4，推送到远程仓库的命令变化成：
// 添加到版本控制
$ git add .

//提交到本地git仓库，" "中的信息最好能体现更改了什么，例如add,updated等。
$ git commit -m "add"

//变化的地方
//推送到Gitee
$ git push gitee master
//推送到GitHub
$ git push github master

5，如果报错则先拉取下来再推送，拉取命令为：
 //从 Gitee上拉取下来
 $ git pull gitee master
 //从 GitHub 上拉取下来
 $ git pull github master

6, 强制推送到GitHub上指令，
//使用强制推送不会对项目造成影响,一般不推荐强制push。初始化的新项目可以用的。
 $ git push github master -f

```

- 实操如下：
  <div align='center'><img src=./images/git-Demo/git-Demo_2019-12-30-23-56-43.png width='80%'/></div><br/>

- github上下载项目，直接：git clone 复制的地址

# 使用常用命令（表格命令前要加git）

1.个人本地使用

<br/>

| 行为         | 命令                 | 备注                                                   |
|------------|--------------------|------------------------------------------------------|
| 初始化        | init               | 在本地的当前目录里初始化git仓库                                    |
|            | clone 地址           | 从网络上某个地址拷贝仓库\(repository\)到本地                        |
| 查看当前状态     | status             | 查看当前仓库的状态。碰到问题不知道怎么办的时候，可以通过看它给出的提示来解决问题             |
| 查看不同       | diff               | 查看当前状态和最新的commit之间不同的地方                              |
|            | diff 版本号1 版本号2     | 查看两个指定的版本之间不同的地方。这里的版本号指的是commit的hash值               |
| 添加文件       | add \-A            | 这算是相当通用的了。在commit之前要先add                             |
| 撤回stage的东西 | checkout \-\- \.   | 这里用小数点表示撤回所有修改，在\-\-的前后都有空格                          |
| 提交         | commit \-m "提交信息"  | 提交信息最好能体现更改了什么                                       |
| 删除未tracked | clean \-xf         | 删除当前目录下所有没有track过的文件。不管它是否是\.gitignore文件里面指定的文件夹和文件  |
| 查看提交记录     | log                | 查看当前版本及之前的commit记录                                   |
|            | reflog             | HEAD的变更记录                                            |
| 版本回退       | reset \-\-hard 版本号 | 回退到指定版本号的版本，该版本之后的修改都被删除。同时也是通过这个命令回到最新版本。需要reflog配合 |

<br/>

2.个人使用远程仓库：

<br/>

| 行为        | 命令                                   | 备注                               |
|---------|------------------------------------|--------------------------------|
| 设置用户名     | config \-\-global user\.name "你的用户名" |                                  |
| 设置邮箱      | config \-\-global user\.email "你的邮箱" |                                  |
| 生成ssh key | ssh\-keygen \-t rsa \-C "你的邮箱"       | 这条命令前面不用加git                     |
| 添加远程仓库    | remote add origin 你复制的地址             | 设置origin                         |
| 上传并指定默认   | push \-u origin master               | 指定origin为默认主机，以后push默认上传到origin上 |
| 提交到远程仓库   | push                                 | 将当前分支增加的commit提交到远程仓库            |
| 从远程仓库同步   | pull                                 | 在本地版本低于远程仓库版本的时候，获取远程仓库的commit   |

<br/>

3，可以用一张图直观地看出以上主要的命令对仓库的影响。

- 示图一：

<div align='center'><img src=./images/git-Demo/git-Demo_2023-02-20-11-46-23.png width='100%'/></div><br/>

例如需要回退最近的一个commit，我们的操作命令参考如下：
参考地址：<https://blog.csdn.net/m0_61682705/article/details/127831663>

```shell
git status //查看当状态
git log //显示从近到远的日志记录，按向下键来查看更多，按 ​​​Q​​​ 键退出查看日志
或者
git log --pretty=oneline //简洁显示日志记录,按 ​​​Q​​​ 键退出查看日志
git reset --mixed 版本号 //来源于上面的日志id，取第二个即commit前一个id 例如：dd98badd0b51d0b494fd959af91386f175b98986

```

- 示图二：

<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-16-09-17.png width='80%'/></div><br/>

下面图片来自：工作区和暂存区 - 廖雪峰的官方网站 （做了点修改）
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-16-09-54.png width='80%'/></div><br/>

# git学习参考教程

- [廖雪峰的Git教程](https://www.liaoxuefeng.com/wiki/896043488029600)
- [1小时学会Git](https://www.cnblogs.com/best/p/7474442.html#_label0)

# 工作实践中有用到的

### 1，git拉取远程某分支强制覆盖本地代码分支（与git远程仓库保持一致,例如master分支）

第一种方式: reset --hard 参数

```shell
git fetch --all  
git reset --hard origin/master
git pull

# 命令说明：

# 第一个是：拉取所有更新，不同步；
# 第二个是：本地代码同步线上最新版本(会覆盖本地所有与远程仓库上同名的文件)；
# 第三个是：再更新一次（其实也可以不用，第二步命令做过了其实）

```

单条执行
`git fetch --all &&  git reset --hard origin/master && git pull`

> 说明：命令连接符 && 的意思是： 前一条命令执行成功才执行后一条命令。
扩展命令连接符 ;; 的意思是：不论前一条是否执行成功都继续执行后一条命令。

第二种方式(情况)：pull --force参数

有的时候，已经知道远程分支与本地分支有不同的commit，比如本地分支有一个临时的commit,远程分支并没有。是不能简单执行git pull的，会报错。
此时如果只是想放弃本地的临时提交，强制将远程仓库的代码覆盖到本地分支。就要用到--force参数，强制拉取功能，命令格式如下：

```shell
$ git pull --force  <远程主机名> <远程分支名>:<本地分支名>
# 示例：
$ git pull --force origin dev:dev
```

### 2，不同分支的合并命令;(例如我们要实现master 合并到 develop)

- 如果本地有未提交的代码，首先要将代码提交到本地git仓库，无论你当前在哪个分支

```shell
git add .
git commit -m "提交的备注信息XXXX"

```

- 查看当前分支的状态`git status`

```shell
F:\ideaHupZdSass\yingcai-manager-server>git status
On branch master
Your branch is up to date with 'origin/master'.

nothing to commit, working tree clean

```

- 如果不是master分支，则用以下命令切换到master 分支
  
```shell
git checkout master

```

- 拉取当前分支最新的远程仓库代码 `git pull`

```shell
F:\ideaHupZdSass\yingcai-manager-server>git pull
warning: redirecting to http://gitlab.wangxiao.cn:9090/wuchengkun/yingcai-manager-server.git/
Already up to date.

```

- 如果master 分支拉取最新代码时，有冲突，则需要先手动解决冲突，然后提交到本地git仓库；

```shell
git add .
git commit -m "解决冲突"

```

- 切换到你所在分支dev (develop)

```shell
F:\ideaHupZdSass\yingcai-manager-server>git checkout develop
Switched to branch 'develop'
Your branch is up to date with 'origin/develop'.

```

- 合并master到自己的分支dev上

```shell
git merge master
```

- 此处如果有冲突会给出提示哪个文件有冲突，修改冲突文件后再继续下面的步骤。
将master上合并的文件add和commit到自己的dev分支

```shell
git add . 

git commit -m "merge master"
```

- 将自己分支dev上的代码提交到远程

```shell
git push

# 或者

git push origin develop （develop与远程git仓库分支同名，origin为默认的远程仓库名称）

# 或者

# 将本地的develop分支强制推送到origin主机，一般不用此场景
# git push -u origin develop

```
