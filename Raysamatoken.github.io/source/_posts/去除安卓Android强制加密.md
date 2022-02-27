---
title: 去除安卓Android强制加密
date: 2022-02-27 12:35:30
tags:
---

要禁止强制加密需要修改/vendor/etc/fstab.qcom，而对于出厂为Android 11的设备都是Virtual A/B结构，vendor分区是一个只读的动态分区。所以要么自己修改编译内核去掉强制加密的函数，要么改vendor.img
  
  解压卡刷包中的payload.bin，使用payload_dumper提取vendor.img
如果已经刷入系统了，可以重启到recovery mode，enable adb并且挂载系统分区Mount system之后，直接提取

```adb pull /dev/block/mapper/vendor_a vendor.img``` # A分区
```adb pull /dev/block/mapper/vendor_b vendor.img``` # B分区  

当然这两个文件其实都是同一个文件的链接，在我的设备上是一个名为dm-4的块，所以
```adb pull /dev/block/dm-4 vendor.img```
同样可以正常工作  

使用十六进制编辑器（比如010 editor）打开提取的vendor.img，搜索```/dev/block/bootdevice/by-name/userdata```，这些字符串在fstab.qcom文件中。
向下滑，定位fileencrypt，选择这些字符串，替换为0x20（也就是blank space），我没有保留vendor.img的副本，所以使用fstab.qcom来演示：
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/Snipaste_2022-02-18_13-00-53.jpg)
替换之后：
![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/Snipaste_2022-02-18_13-01-19.jpg)  
重启到bootloader模式，刷入修改后的vendor.img
fastboot flash vendor vendor.img
最后，去除dm verify，简单点就直接刷入Magisk.zip，重启即可。
```adb shell
echo 'KEEPVERITY=false' >/cache/.magisk
echo 'KEEPFORCEENCRYPT=false' >>/cache/.magisk
exit
adb sideload Magisk.zip```
还有一个未测试的方法，如果要手动去除dm verify，可以打开vbmeta.img，ctrl + g跳转到120字节处，修改为00 00 00 03即可。