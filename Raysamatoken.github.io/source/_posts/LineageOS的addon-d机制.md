---
title: LineageOS的addon.d机制
date: 2022-01-23 17:30:13
tags:
---
&ensp;以下场景假设正在使用CM系(cm , los , havoc , crdroid , rr......)ROM。  

&ensp;也许你曾经疑惑过，为什么我塞进系统的APP在ota后就没了，但是为什么gapps在ota后还在？为什么我的面具在ota后就掉了，但另一个手机在ota后面具还在？(该问题暂不作解释，面具的刷入流程有一些奇怪的判断逻辑)  

&ensp;下面，有请辛勤的幕后工作者，模块化备份工具，backuptool登场。  
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123173824.png)
&ensp;首先，我们知道，Android系统更新分为A/B更新和非A/B更新。
&ensp;A/B更新：闪存上有两套系统分区，通过update_engine把新的系统写入到另一套分区，重启后引导另一套分区。
&ensp;非A/B更新：闪存上只有一套系统分区，通过recovery把新的系统写入到当前的系统分区，重启后也是引导这套分区。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123173913.png)
所以，问题来了：无论哪种方案，开机后你能看到的系统分区都是在更新被覆盖过的，所以你塞进系统的APP没了似乎也很好理解(吧？)。但是！gapps也是被rec直接塞进/system里的，为什么更新完之后还在呢？咋们换个思路吧，如果在更新之前把塞进系统的东西拿出来，更新完之后再放回去是不是可以呢？对！这就是理论上backuptool要干的活。LineageOS的构建系统提供了backuptool的支持，集成了这个功能的ROM会通过backuptool[_ab].sh脚本运行/system/addon.d中的脚本。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174017.png)
因为安卓手机有两种更新方式，所以Los提供了两种addon.d脚本支持方式：addon.d-v1(用于非A/B更新)和addon.d-v2(用于A/B更新)。addon.d-v3是在前两者基础上增加挂载product , vendor , system_ext分区
v1方案：
这里解压一个recovery卡刷包，查看刷机脚本(```META-INF/google/android/updater-script```)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174057.png)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174122.png)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174137.png)  
可以看到，在刷入镜像前以```$1=backup```运行```backuptool.sh```，刷入后以```$1=restore运行backuptool.sh```。(刷机包中的install目录在run_program前已被刷机脚本解压到/tmp并设置好权限)
现在，我们看看los提供的示例文件```/system/addon.d/50-lineage.sh```
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174237.png)
其实对于addon.d脚本，并没有什么太明确的格式要求(又不是不能用.jpg滑稽)，backuptool只是提供了一组预定义的环境变量和函数(如果你的脚本引入了backuptool.functions的话)简化编写。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174303.png)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174317.png)
backuptool.functions提供了四个函数，一般来说backup_file(把文件存放到暂存区)和restore_file(把文件从暂存区拷到指定位置)是最常用的。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174340.png)
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174402.png)
backuptool.sh负责备份还原及运行/system/addon.d里的脚本，同时提供$C(暂存区)$V(发行版本)$S(system分区挂载点)三个环境变量。
v2方案：
总体上说addon.d-v2和v1提供的函数和变量名是基本一样的。但有几点需要注意：
1：addon.d-v2的backuptool_ab.sh运行时间是在刷入完成后，由backuptool_postinstall.sh依次执行backup和restore
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174431.png)
/postinstall是aosp的标准目录，作用是如果新系统刷好后需要“预处理”，update_engine会把新的系统挂载到/postinstall再处理
2：在脚本中需要写上注释“# ADDONS_VERSION=2”，否则会被判定为不兼容而不执行。
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/20220123174507.png)
最后，我们回到开头的问题，看看opengapps除了把gapps放进系统以及删除冲突的软件之外还做了什么？
首先，在/system/addon.d里放了一个叫70-gapps.sh的文件并设置了可执行权限，把他的文件列表写到list_files函数里。并把删除冲突软件包的命令写到pre-restore阶段，把建立软链接的命令写到post-restore阶段。相当于就是把gapps重新给刷一次。
很明显，你随手塞进系统的APP并没有进行这种操作。所以在ota时系统不会备份和还原你的APP，于是......  
下面，开始编写你的第一个addon.d脚本吧(括号里的字是解释，不用写)：  
```#!/sbin/sh
(为了兼容性，请使用/sbin/sh作为解释器)
# ADDONS_VERSION=2
(声明此脚本支持addon.d-v2)
. /tmp/backuptool.functions
(为了兼容性，请引入/tmp/backuptool.functions)
list_files() {
cat <<EOF
XXXXX
XXXXX(请把"XXXXX"替换成你要备份的系统文件相对于/system的路径，比如你要备份/system/xbin/busybox就写xbin/busybox既可。如果是多个文件就一行一个)
EOF
}
case "$1" in
backup)
list_files | while read FILE DUMMY; do
backup_file $S/"$FILE"
done
;;
restore)
list_files | while read FILE REPLACEMENT; do
R=""
[ -n "$REPLACEMENT" ] && R="$S/$REPLACEMENT"
[ -f "$C/$S/$FILE" ] && restore_file $S/"$FILE" "$R"
done
;;
pre-backup)
# Stub
;;
post-backup)
# Stub
;;
pre-restore)
# Stub
;;
post-restore)
# Stub
;;
esac

(esac下面记得留一个空行！！！)  
```
写完了，保存(编码UTF-8，注意文件名要以".sh"结尾)。挂载system读写，把文件拷进/system/addon.d里，所有者/用户组0，权限0755
