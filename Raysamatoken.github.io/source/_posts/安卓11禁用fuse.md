---
title: 安卓11禁用fuse
date: 2022-01-15 21:35:18
tags:
---

安卓11禁用Fuse，大幅提高IO性能
谷歌处于某种原因考虑，在安卓11中禁用了sdcardfs.
有测试指出fuse比sdcardfs慢了30多倍，显而易见的，氢11中，相册的加载速度变得异常缓慢.
经过对AOSP 中StorgeMangerService的分析，可利用一下prop属性简单的禁用fuse.

```
persist.sys.fuse=false
persist.fuse_sdcard=false
persist.sys.fuse.default_fuse_enabled=false
persist.sys.fflag.override.settings_fuse=false
persist.device_config.storage_native_boot.fuse_enabled=false
ro.sys.sdcardfs=true
```

禁用后，mount属性如下

![](https://raw.githubusercontent.com/Raysamatoken/hexofile/main/img/49522268010914692e38969756e2023b.jpg)

可能在部分魔改UI不起作用，MIUI测试不通过

也可以刷模块：

https://raw.githubusercontent.com/Raysamatoken/hexofile/main/file/%E5%90%AF%E7%94%A8sdcardfs_b5.zip