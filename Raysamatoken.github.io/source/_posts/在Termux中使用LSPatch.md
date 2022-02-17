---
title: 在Termux中使用LSPatch
date: 2022-01-31 17:25:11
tags:
---

1. 安装最新版 Termux
下文中使用的版本为 0.118-debug 版本的 Termux，较老的版本可能无法安装 openjdk。由于签名不同，安装前请先卸载原来的Termux。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1792764_df4c70c6_4435_9211_827%401080x947.jpeg.m.jpg)  

1. 配置 apt 源
首先将 Termux 的默认源地址替换为清华 tuna 源，保证速度。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1792764_f7d657e7_4435_9214_394%401080x221.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1792764_f58fadeb_4435_9223_580%401080x225.jpeg.m.jpg)
命令：
```sed -i 's@^\(deb.*stable main\)$@#\1\ndeb https://mirrors.tuna.tsinghua.edu.cn/termux/termux-packages-24 stable main@'; $PREFIX/etc/apt/sources.list```  

1. 安装openjdk
执行 pkg install openjdk-17
在跑完一长串代码以后，执行java -version确认是否安装成功。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1792764_76dbe32a_4435_9226_142%401080x136.jpeg.m.jpg)
在开始操作之前记得赋予Termux全部文件管理权限，便于之后操作。
然后cd到lspatch.jar文件的所在目录即可。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1792764_bf337669_4435_9236_339%401080x670.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1792764_ed2f12e1_4435_924_903%401080x368.jpeg.m.jpg)