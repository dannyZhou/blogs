---
layout: post
title: decrypt hd3000 exe file
date: 2020-01-27
categories: decrypt
tags: exe
---

* content
{:toc}

### 序
今天公司让我破解一个软件 hd3000 组播软件，具体链接不放出。把一些思路和资料写下来。

### 参考资料
* [看雪工具](https://tools.pediy.com/win/decompilers.htm)
* [52破解](https://www.52pojie.cn/)
* [upx](https://github.com/upx/upx)

### 环境
1. 52破解虚拟机，xp sp3
2. upx 软件
3. 52破解虚拟机中的破解软件
4. 火绒剑

### 教程以及关键
1. upx -d 可以脱壳 upx 软件，因为 upx 本身是为了压缩，而不是为了加密，但是由于软件被 upx 压缩后，又做了其他工作，所以这个命令失败。
2. 52破解 虚拟机里面很多关于 upx 的软件，都试了是没有办法使得它成功脱出（后来发现其实脱壳已经成功，只不过软件有自校验），运行的时候一直报错。
3. 将原始软件只更改了一个无所谓的字符，发现软件也运行失败，这说明软件本身有自校验。 upx 本身没有自校验。这说明软件的校验来自于软件内部。
4. 只更改了一个字符，说明软件对自身进行了自校验。那样，必然有读取本身这个操作。
5. 用火绒剑来监听软件的访问，发现了软件对自己读取的逻辑。根据火绒剑的堆栈跟踪，加断点，运行之后和原始的软件运行逻辑进行对比，发现某个逻辑是将返回的 eax-0x77 之后和 0 对比，如果大于 0 和小于 0 逻辑是不一样的。更改之后发现软件可以正常运行了。
6. 下面是进行破解，首先，想通过弹出的 MessageBoxA 入手，失败。
7. 仔细观察软件，发现没有输入激活码的地方，只给了电脑的特征码。wireshark 抓包没有发现任何软件发出的网络请求。推测可能是读取文件进行的验证。
8. 再次使用火绒剑进行过滤分析，发现软件读取了 reg.ini 和 reg.dat, 并且读取失败因为软件中根本不存在这两个文件。
9. 尝试添加一个带有普通数据的 reg.ini, 发现软件逻辑走的逻辑更改了。
10. 最后找到两个比较返回值的地方，将其 nop 掉，软件破解成功。