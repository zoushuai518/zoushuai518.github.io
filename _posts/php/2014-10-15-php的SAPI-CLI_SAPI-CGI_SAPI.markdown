---
layout: post
title: "php的SAPI CLI SAPI、CGI SAPI"
category: php
comments: true
date:   2014-10-15 16:35:51
---

首先一个问题：在命令行下执行：php -r 'echo 12;'

控制台会打印出 12；

这个过程不是很奇妙么，我输入的是shell命令，但是执行的却是php脚本。php脚本执行完成之后的输出还能在控制台输出。

那在这个shell命令（控制台命令）和php中间一定有一种接口，能将shell的参数，代码，等转换成php，然后将php的输出转换成shell的输出。这个接口就叫做SAPI（Server Application Programimg Interface）。它就相当于PHP外部环境的代理器。

那么由于PHP可以应用在终端上，也可以应用在Web服务器中，所以呢，应用在终端上的SAPI就叫做CLI SAPI，应用在Web服务器中的就叫做CGI SAPI。在windows下安装php你会看到两个exe：php.exe和php-cgi.exe这个就对应的是这两种SAPI。再比如，在控制台上使用php -v，你就会发现PHP的版本信息中有个（cli）标示，就代表你这里的php应用程序使用的是cli SAPI。

关于CLI SAPI：[手册上有很详细的说明：](http://php.net/manual/zh/features.commandline.php)
