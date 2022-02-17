---
title: 安卓应用lspatch注入Xposed框架
date: 2022-01-31 17:17:45
tags:
---

Lspatch项目地址:
https://github.com/LSPosed/LSPatch
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/4231255_3086f105_7468_2643%401747x884.jpeg.m.jpg)

2 准备工作：（lspatch目前是在电脑端使用的，接下来的操作都是基于Windows11，其它版本未必不可）  

2.1 下载java11 安装并配置环境
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/4231255_adb48855_7468_2645%401649x808.jpeg.m.jpg)

2.2 下载lspatch-release
这个版本打包bilibili实测可用（因为开发者正在开发，其它版本问题未知）  

2.3 下载需要的apk
包括要打包（也就是你想破解）的apk（和你想使用的xposed应用）
举个例子：分别为1.apk和1_xposed.apk（最好都用英文命名，防止出现问题）  


2.4 移动至同一文件夹
（实际上这一步骤只是用来让我们运行jar时不用输入绝对路径）
我们先将lspatch-release解压到一个文件夹lspatch，然后将需要破解的应用和xpose应用移动至lspatch文件夹
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/4231255_05d0fb20_7468_2647%401435x631.jpeg.m.jpg)

3 开始打包
3.1 打开CMD或者PowerShell窗口
如果此时命令行窗口中的默认路径不是lspatch文件夹的路径，那么请更改至该路径或者将之后命令中用到的路径都改为绝对路径  

3.2 输入命令
输入 ```java -jar lspatch.jar -h``` 获取帮助信息
![](https://raw.githubusercontent.com/Raysamatoken/hexo/main/img/20220131172224.png)
不内置xposed应用作为模块：
```java -jar lspatch.jar``` 要打包的应用的文件名.apk
例：```java -jar lspatch.jar 1.apk```
内置xposed应用作为模块：
```java -jar lspatch.jar``` 要打包的应用的文件名.apk -m 要内置的xposed应用的文件名.apk
例：``` java -jar lspatch.jar 1.apk -m 1_xposed.apk```
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/4231255_a8189a47_7468_2651%402350x1277.jpeg.m.jpg)
特殊的命令查看帮助信息，不同版本的帮助信息不同，但命令应该是一样的，可能有增加或者减少  


4 查看文件
返回lspatch文件夹，这时文件夹里生成了一个打包好的apk
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/4231255_05daef73_7468_2653%401406x190.jpeg.m.jpg)
