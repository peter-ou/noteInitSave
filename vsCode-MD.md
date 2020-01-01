# VSCode-Markdown图片复制插件路径配置 

- 我个人比较喜欢 Paste Image，方便一些
  
[Paste Image 官方文档](https://marketplace.visualstudio.com/items?itemName=mushan.vscode-paste-image)

[Paste Image 插件的GitHub地址](https://github.com/mushanshitiancai/vscode-paste-image)

- 方式一： 工作区中找到.vscode下的setting.json 设置粘贴图片的路径参数

```
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


