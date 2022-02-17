---
title: 出厂安卓11机型解锁system分区教程
date: 2022-02-16 09:20:49
tags:
---
出厂安卓11的设备都是vab架构，具体百度吧，我也不是很清楚vab架构。要解锁system，原理就是去除vbmeta检验就可以了，本教程就是去除vbmeta检验(简称AVB2.0检验)，已达到解锁system分区的目的，任意修改system分区。  

要去除AVB2.0检验，就要修改/vendor/etc/fstab.qcom文件，删除里面的avb字样的代码，就可以了。但是，再没解锁system分区的时候，是无法修改/vendor/etc/fstab.qcom文件的，而要解锁system分区，就是要修改/vendor/etc/fstab.qcom文件。所以，似乎陷入了一个死循环？  

所以，要解决这个问题，就只能把vendor分区提取出来，修改，然后再刷回去，就可以了。思路有了，该怎么做呢，跟着我的步伐，走  

第一步，安装termux软件：
pan.baidu.com/s/1-QsaVYiA596shnk6cJvZiw 提取码:4bc6  
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_b5187d2e_8439_0534%401080x2400.jpeg.m.jpg)  

第二步，安装解包打包工具-DNA：
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_8978425e_8439_0541%401080x2400.jpeg.m.jpg) 

第三步，提取vendor分区：pan.baidu.com/s/1QctyPF9Kuw3KAQGhylf-kA 提取码:7l7w
有两个sh脚本，安装个MT管理器，以root的方式执行即可提取出来，提取出来的文件在/sdcard/vendor.img。注意，如果你当前是A分区就用A分区那个，如果是B分区就用B分区那个。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_d8a6a061_8439_0543%401080x2400.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_effbbc0f_8439_0545%401080x2400.jpeg.m.jpg)
这个vendor.img就是提取出来的镜像文件
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_1e9e1b19_8439_0547%401080x2400.jpeg.m.jpg)
DNA工具输入0新建一个工程
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_0b25eb67_8439_0549%401080x2400.jpeg.m.jpg)
名称任意即可。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_53881fac_8439_055%401080x2400.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_1297c174_8445_0702%401080x2400.jpeg.m.jpg)
把提取出来的vendor.imf文件，复制到DNA工具新建的工程中：路径/data/data/com.termux/files/home/ubuntu/root/DNA/Errors_test/
这个Errors_test就是你刚刚新建出来的工程名字。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_36400bcb_8445_0704%401080x2400.jpeg.m.jpg)
设置文件权限为666：
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_4d80ee01_8445_0706%401080x2400.jpeg.m.jpg)
第四步，解包vendor：DNA工具中，输入04，解包img文件
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_e8780a7b_8445_0707%401080x2400.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_69caa5a2_8445_0709%401080x2400.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_0e768453_8445_0711%401080x2400.jpeg.m.jpg)
等待一点时间，解包完成。

第五步，修改vendor/etc/fstab.qcom文件，去掉里面的avb字样代码。
这里就是解包vendor出来的文件了，修改etc下的fstab.qcom文件，删除所有avb的字样，即可去除avb2.0检验。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_bde2240c_8445_0713%401080x2400.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_66dd0e0e_8445_0716%401080x2400.jpeg.m.jpg)

我给你们演示的这个系统，是我已经去除了avb检验了的，所以里面已经没有avb字样了。

需要注意的是，这种avb字样是不需要去除的，保留即可。其他的avb字样都删除掉。其他的好像是avb,avb=system之类的。只留下这7个avb代码。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_cfcdc7d0_8445_0718%401080x2400.jpeg.m.jpg)
删除完毕之后，打包刷入就好了。
需要注意的是，直接打包有可能打包失败，原因是大小不对，我们只需要改一下打包大小即可。

需要注意的是，大小不是任意的，这取决于你的vendor分区大小。打包出来的大小不能＞你vendor分区的大小。那个打包大小的文件，单位是字节。1024字节=1KB。打包大小配置文件：configs/vendor_size.txt。

改好大小后，删除想要删除的文件之后，开始打包。

![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_f6b8d27f_8450_4131%401080x2400.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_a6bea200_8450_4133%401080x2400.jpeg.m.jpg)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_d0fcb654_8450_414%401080x2400.jpeg.m.jpg)
这个vendoe_dna.img就是打包出来的文件了。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_3d597402_8450_4143%401080x2400.jpeg.m.jpg)


打包好了，接下来就是刷入到vendor分区即可。怎么刷，方法很多，以我机型为例。
用adb工具刷入：
进入fastboot模式，用fastboot reboot fastboot进入fastbootd模式，再用命令fastboot flash vendor_a 你打包出来的那个img文件

重新开机后，就可以删除修改vendor或者system分区的东西了。
