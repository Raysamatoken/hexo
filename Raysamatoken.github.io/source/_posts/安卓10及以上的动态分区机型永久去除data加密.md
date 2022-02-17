---
title: 安卓10及以上的动态分区机型永久去除data加密
date: 2022-02-16 09:32:37
tags:
---

首先，我们需要在目录【/vendor/etc/】中找到文件【fstab.qcom】，有的机型可能不叫这个名字，总之就是带有【fstab】字样的所有文件，都需要修改。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_7277df1d_0814_1101_134%401080x2400.jpeg.m.jpg)
打开【fstab】字样的文件，里面是这样的：
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_2423909d_0814_1105_917%401080x2400.jpeg.m.jpg)
找到【userdata】这一行代码，里面有类似于这样的代码```fileencryption=ice,wrappedkey,keydirectory=/metadata/vold/metadata_encryption,```，如图：
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/1294855_37b2d9e0_0814_1114_962%401080x2400.jpeg.m.jpg)
注意，每个机型可能代码不是完全一模一样，但是基本上都差不多长这个样子。然后就是删掉他【注意上图我选中的代码中，末尾包含了一个逗号的，不要删漏了】。然后就是保存，然后进行一次data格式化后，再开机时，data便不会被加密了。  

格式化data方法一：进入rec，格式化data(需要刷入yes的那种)。
格式化data方法二：进入fastboot(小米动态非vab机型和魅族动态分区机型)，输入命令【fastboot format userdata】和【fastboot format metadata】
格式化data方法三：进入fastbootd(小米动态vab机型)，输入命令【fastboot format userdata】和【fastboot format metadata】

data加密的判断方式：格式化data后，第一次开机时，读取【/vendor/etc/fstab.qcom】文件，该文件是否存在上面我们删除的代码，如果存在，则对data进行加密操作，若不存在，则不对data进行data操作。
