---
layout: post
title: "济宁惠普实训基地虚拟机安装 gentoo 的经历"
date: 2015-08-29
categories: linux
tags: linux gentoo
---

* content
{:toc}

### 序
> 一直想安装一个 gentoo，但是限于电脑笔记本的配置较低，
> 一直没有实现这个愿望。来到实训基地以后，有了更快的电脑，
> 找了一个星期天，开始了 gentoo 安装的旅程。
> 这里主要是列举了在安装过程中所遇到的问题。

### 安装环境
 宿主机：windows7 Intel(R) Core(TM) i5-4590 CPU @3.30GHz 3.30GHz
		内存 8GB
 		vmware player
 虚拟机：Intel(R) Core(TM) i5-4590 CPU @3.30GHz 3.30GHz
		内存 8GB

### 参考资料
 [gentoo website](https://www.gentoo.org/)
 
 [gentoo wiki install guide](https://wiki.gentoo.org/wiki/Handbook:X86)

### 遇到的问题——版本问题
 install 安装光盘是使用的 x86 也就是 32 位的，但是下载的 `stage3` 是 64 位的，解压之后，运行时会出现一个错误。
 `cannot execute binary file : exec format error `。
 `这也给我们以后提了一个醒，如果出现这个错误，很可能是软件的架构和系统的结构不是一个。

### USE 的官方说法（翻译）
 USE 是 gentoo 提供给用户的一个最有用的变量，很多的程序可以带着或者不带着一些选项进行编译，编译出来的文件也可以支持或者不支持一些选项。
 例如：一些程序可以带着 gtk 或者 qt 的支持进行编译，另一些可以添加或者删除 SSL 的支持，一些程序甚至可以用 `svgalib` 进行编译而不是 `x11`。
 很多的 linux 发行版编译他们的包让他们的包支持的越多越好，这样增长了程序的尺寸和运行开始的时间，和一个很大的依赖量。
 而 gentoo 的用户可以定义 USE 来决定到底需要编译哪些可选项。

### 安装感想
 编译的时间太长，即使是全开马力。而且一旦 USE 标签设置错误，将会重新编译。相比之下更喜欢 archlinux 的安装方式和 AUR 的存在。
