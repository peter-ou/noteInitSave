## 第一次使用Git上传本地项目到github上的实现

<!-- GFM-TOC -->

- [git和github的相关联实现](#git和github的相关联实现)
- [git如何提交内容到GitHub仓库中](#git如何提交内容到github仓库中)


# git和github的相关联实现

1，下载Git软件：https://git-scm.com/downloads，据说ios自带的有git软件，这个我就不太清楚了。

2，下载之后安装就很简单了，一路下一步就可以了。安装完成后鼠标右击和者开始->程序会出现（如下图），打开Git Bash，进入bash界面。
<div align='center'><img src=./images/git-Demo/git-Demo_2019-12-31-12-50-58.png width='80%'/></div><br/>

3，执行git init 建好git 的本地仓库（可以有多个）,就是在本地创建一个版本库（即文件夹），通过git init把它变成一个Git仓库；
```
git init
```
假如我们建好的本地git仓库是： /d/studyNote/noteInit (master)

4，配置本地git信息
```
//说明：双引号中需要你的用户名，这个可以随便输入，比如“zhangsan”
git config --global user.name "user.name"  ##回车
//双引号中需要输入你的有效邮箱，比如“12131312@qq.com”
git config --global user.email "yourmail@youremail.com.cn"  ##回车
// 查看配置结果 
git config –list  ##回车  
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

10, 把要上传的到GitHub的文件夹放入 本地git仓库：D:\studyNote\noteInit 中

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



# git如何提交内容到GitHub仓库中

- 命令如下：
```
//add只是将文件或目录添加到了index暂存区
//使用commit可以实现将暂存区的文件提交到git本地仓库，[message]为备注信息
// 将本地git仓库内容提交到关联好了的 GitHub 仓库 
git add .
git commit -m "message"
git push origin master
```
- 实操如下：
  <div align='center'><img src=./images/git-Demo/git-Demo_2019-12-30-23-56-43.png width='80%'/></div><br/>

