---
layout: post
title: install supersu via adb with root
date: 2020-01-17
categories: android
tags: java tomcat
---

* content
{:toc}

### 序
ttl 下连接的 android 盒子一般都是 root 权限，现在想用移动魔百盒 cm201-2 进行安装 supersu

### 参考资料
* [install-supersu-via-adb github](https://github.com/spff/install-supersu-via-adb)
* [install-supersu-via-adb](http://selvaline.blogspot.tw/2015/09/how-to-install-supersu-on-android-via.html)
* [super su download website](https://download.chainfire.eu/696/supersu/)

### 环境
1. 移动 cm201-2 
2. android 4.4 
3. ttl adb 为 root

### 简单教程
1. 将所有的文件放到该放的位置
2. 重启机顶盒
3. 打开 supersu
4. 更新后重启

### 关键
```
 # API  source            target               chmod  chcon            required  
 #  
 # 7-19 common/Superuser.apk     /system/app/Superuser.apk      0644  u:object_r:system_file:s0  gui  
 # 20+  common/Superuser.apk     /system/app/SuperSU/SuperSU.apk   0644  u:object_r:system_file:s0  gui  
 #  
 # 17+  common/install-recovery.sh  /system/etc/install-recovery.sh   0755  *1             required  
 # 17+                 /system/bin/install-recovery.sh   (symlink to /system/etc/...)    required  
 # *1: same as /system/bin/toolbox: u:object_r:system_file:s0 if API < 20, u:object_r:toolbox_exec:s0 if API >= 20  
 #  
 # 7+  ARCH/su            /system/xbin/su           *2   u:object_r:system_file:s0  required  
 # 7+                 /system/bin/.ext/.su        *2   u:object_r:system_file:s0  gui  
 # 17+                 /system/xbin/daemonsu        0755  u:object_r:system_file:s0  required  
 # 17+                 /system/xbin/sugote         0755  u:object_r:zygote_exec:s0  required  
 # *2: 06755 if API < 18, 0755 if API >= 18  
 #  
 # 19+  ARCH/supolicy         /system/xbin/supolicy        0755  u:object_r:system_file:s0  required  
 # 19+  ARCH/libsupol.so       /system/lib(64)/libsupol.so     0644  u:object_r:system_file:s0  required  
 #  
 # 17+  /system/bin/sh or mksh *3   /system/xbin/sugote-mksh      0755  u:object_r:system_file:s0  required  
 # *3: which one (or both) are available depends on API  
 #  
 # 21+  /system/bin/app_process32 *4 /system/bin/app_process32_original 0755  u:object_r:zygote_exec:s0  required  
 # 21+  /system/bin/app_process64 *4 /system/bin/app_process64_original 0755  u:object_r:zygote_exec:s0  required  
 # 21+  /system/bin/app_processXX *4 /system/bin/app_process_init    0755  u:object_r:system_file:s0  required  
 # 21+                 /system/bin/app_process       (symlink to /system/xbin/daemonsu) required  
 # 21+               *4 /system/bin/app_process32      (symlink to /system/xbin/daemonsu) required  
 # 21+               *4 /system/bin/app_process64      (symlink to /system/xbin/daemonsu) required  
 # *4: Only do this for the relevant bits. On a 64 bits system, leave the 32 bits files alone, or dynamic linker errors  
 #   will prevent the system from fully working in subtle ways. The bits of the su binary must also match!  
 #  
 # 17+  common/99SuperSUDaemon *5   /system/etc/init.d/99SuperSUDaemon 0755  u:object_r:system_file:s0  optional  
 # *5: only place this file if /system/etc/init.d is present  
 #  
 # 17+  'echo 1 >' or 'touch' *6   /system/etc/.installed_su_daemon  0644  u:object_r:system_file:s0  optional  
 # *6: the file just needs to exist or some recoveries will nag you. Even with it there, it may still happen.  

```