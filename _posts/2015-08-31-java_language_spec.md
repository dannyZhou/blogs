---
layout: post
title: 由 interface 和 Object 有什么关系引出来的一个 pdf 《java语言和虚拟机规范》
date: 2015-08-30 
categories: java
tags: java
---

* content
{:toc}

### 序
> 今天打算重写一个 Object，charSequence ，但是 在编写 `charSequence.toString()`  的时候出现了问题。

### 起源
今天打算重写 Object，String，CharSequence，并且构建自己的src（胃口好大！）`charSequence.toString()` 
的时候出现了一个问题，`'toString()' in 'com.danny.java.lang.CharSequence' clashes with 'toString()' in 'java.lang.Object'; attempting to use incompatible return type`
接着印发出一个问题： Interface 和 Object 是什么关系？

### 官方文档
在搜索了部分资料以后，暂时定位到了一个文档，就是 
*[Java Language and Virtual Machine Specifications](https://docs.oracle.com/javase/specs/index.html)*。
这个的中文名字应该是叫做《java 语言和虚拟机规范》了吧。
这是一个规范，具体细节下面介绍吧。在这个文档的第九章，有这样一段话。
_if an interface has no direct superinterfaces, then the interface implicitly declares a public abstract member method m 
with signature s, return type r, and throws clause t corresponding to each public instance method m with signature s, reutrn type r, 
and throws clause t declared in Object, unless a method with the same signature, same return type, and a compatible throws clause is 
explicitly declared by _th interface.