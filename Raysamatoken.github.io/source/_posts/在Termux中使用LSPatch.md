---
title: 在Termux中使用LSPatch
date: 2022-01-31 17:25:11
tags:
---

1. 安装最新版 Termux
下文中使用的版本为 0.118-debug 版本的 Termux，较老的版本可能无法安装 openjdk。由于签名不同，安装前请先卸载原来的Termux。
![](https://raw.githubusercontent.com/Raysamatoken/hexo/main/img/20220131172646.png)  

2. 配置 apt 源
首先将 Termux 的默认源地址替换为清华 tuna 源，保证速度。
![](https://raw.githubusercontent.com/Raysamatoken/hexo/main/img/20220131172731.png)
![](https://raw.githubusercontent.com/Raysamatoken/hexo/main/img/20220131172755.png)
命令：
```sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@'; $PREFIX/etc/apt/sources.list```  

3. 安装openjdk
执行 pkg install openjdk-17
在跑完一长串代码以后，执行java -version确认是否安装成功。
![](https://raw.githubusercontent.com/Raysamatoken/hexo/main/img/20220131172913.png)
在开始操作之前记得赋予Termux全部文件管理权限，便于之后操作。
然后cd到lspatch.jar文件的所在目录即可。
![](https://raw.githubusercontent.com/Raysamatoken/hexo/main/img/20220131172935.png)
![](https://raw.githubusercontent.com/Raysamatoken/hexo/main/img/20220131172954.png)
