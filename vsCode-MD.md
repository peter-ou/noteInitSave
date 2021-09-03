# VSCode-Markdown图片复制插件路径配置 

- 我个人比较喜欢 Paste Image，方便一些
- 我自己的粘贴的快捷键我设置成了 **ctrl+alt+A**
  
[Paste Image 官方文档](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image)

[Paste Image 插件的GitHub地址](https://github.com/mushanshitiancai/vscode-paste-image)

- 方式一：设置 > 用户 > 工作区 中找到.vscode下的setting.json 设置粘贴图片的路径参数。

```shell
{
    "pasteImage.insertPattern": "<div align='center'><img src=${imageFilePath} width='80%'/></div><br/>",
    "pasteImage.namePrefix": "${currentFileNameWithoutExt}_",
    "pasteImage.path": "${currentFileDir}/images/${currentFileNameWithoutExt}",
    "pasteImage.basePath": "${currentFileDir}",
    "pasteImage.forceUnixStyleSeparator": true,
    "pasteImage.prefix": "./",
}
```

- 方式二：直接在插件中设置 相关参数

1， 找到对应的插件，打开其设置
<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2020-01-01-14-23-42.png width='80%'/></div><br/>
  

2，设置参考

- Paste Image: Path 这个设置的是粘贴文件时复制的目标目录
  
<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2020-01-01-14-26-32.png width='80%'/></div><br/>

- Paste Image: Base Path 这个设置会影响到粘贴文件时候 (/image/xxxx.png) 路径的粘贴, 不然会显示 (../images/xxxx.png)

<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2020-01-01-14-27-51.png width='80%'/></div><br/>

- Pates Image: Insert Pattern 这个设置是可以填充 \!\[xxxx]() 的 xxxx 部分，不然默认为空

<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2020-01-01-14-28-26.png width='80%'/></div><br/>

- Paste Image Prefix 这个是图片路径的前缀， 不然图片路径为 ![](images/xxxx.png), 设置这个就是 ![](/images/xxxx.png)

<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2020-01-01-14-29-11.png width='80%'/></div><br/>

- Paste Image的默认粘贴快捷是 ctrl+alt+v
- Paste Image 设置粘贴的快捷键（任何其他软件剪切图片到粘贴板都可以）
 
 > + 第一步找到设置快捷键的地方

<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2020-05-23-12-09-06.png width='80%'/></div><br/>
   
 > + 查询到Paste Image 的快捷键，编辑成自己想要的快捷键
 ，我自己的粘贴的快捷键我设置成了 **ctral+alt+A**

<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2020-05-23-12-10-34.png width='80%'/></div><br/>

# vscode 的终端设置和切换

1,首先肯定是需要打开我们的vscode咯

2,进入终端设置
<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2021-02-20-15-38-40.png width='90%'/></div><br/>

3,修改setting.json 中"terminal.integrated.shell.windows":"XXX"的路径
<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2021-02-20-15-41-54.png width='90%'/></div><br/>

4,cmd 和 bash 的终端切换

<div align='center'><img src=./images/vsCode-MD/vsCode-MD_2021-02-20-15-43-52.png width='90%'/></div><br/>

```shell
oushuncai@DESKTOP-0S8MN7B MINGW64 ~
$ cmd
Microsoft Windows [版本 10.0.19041.746]
(c) 2020 Microsoft Corporation. 保留所有权利。

C:\Users\oushuncai>bash

oushuncai@DESKTOP-0S8MN7B MINGW64 ~
$
```