---
layout: post
title: "linux中stdout,stdin,stderr"
category: Linux
comments: true
date:   2015-01-23 11:32:51
---


stdout, stdin, stderr的中文名字分别是标准输出，标准输入和标准错误。  

在Linux下，当一个用户进程被创建的时候，系统会自动为该进程创建三个数据流，也就是题目中所提到的这三个。那么什么是数据流呢（stream）？我们知道，一个程序要运行，需要有输入、输出，如果出错，还要能表现出自身的错误。这是就要从某个地方读入数据、将数据输出到某个地方，这就够成了数据流。  

因此，一个进程初期所拥有的这么三个数据流，就分别是标准输出、标准输入和标准错误，分别用stdout, stdin, stderr来表示。对于这三个数据流来说，默认是表现在用户终端上的.

- [linux中stdout,stdin,stderr意义](http://blog.sina.com.cn/s/blog_912673ce01013qq9.html)
- [Linux Shell 文件描述符 及 stdin stdout stderr 重定向](http://blog.chinaunix.net/uid-20671208-id-3588619.html)



- [如何在Linux终端里用Shell和C输出带颜色的文字](http://blog.csdn.net/acmee/article/details/6613060)
