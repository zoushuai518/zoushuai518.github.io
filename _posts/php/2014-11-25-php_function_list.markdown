---
layout: post
title: "php常用函数列表"
category: php
comments: true
date:   2014-11-25 10:00:51
---

#####String Functions

- str_pad() 把字符串填充为指定的长度
- sprintf() 把格式化的字符串写入一个变量中
- strip_tags() 函数剥去 HTML、XML 以及 PHP 的标签
- htmlentities() 函数把字符转换为 HTML 实体
- htmlspecialchars() 函数把一些预定义的字符转换为 HTML 实体
- str_replace() 函数使用一个字符串替换字符串中的另一些字符
- strncmp() 函数比较两个字符串

- printf() 输出格式化字符串
- sprintf() 函数把格式化的字符串写入一个变量中
- vprintf() 输出格式化字符串
- fprintf() 将格式化后的字符串写入到流
- vfprintf() 将格式化字符串写入流
- vsprintf() 返回格式化字符串
- fscanf() 从文中格式化输出

- ctype_print() 做可打印字符检测


#####pattern Functions

- split() 用正则表达式将字符串分割到数组中


#####Array Functions

- array_flip() 函数返回一个反转后的数组，如果同一值出现了多次，则最后一个键名将作为它的值，所有其他的键名都将丢失
- shuffle() 函数把数组中的元素按随机顺序重新排列
- sizeof() 函数计算数组中的单元数目或对象中的属性个数


#####File Functions


#####Filesystem Functions

- popen() 函数打开进程文件指针
- proc_open() 执行一个命令，并且打开用来输入/输出的文件指针


#####Calendar Functions


#####Date/Time Functions


#####Db Functions


#####Directory Functions


#####HTTP Functions


#####Image Functions


#####IMAP Funictions


#####LDAP Functions


#####Mail Functions


#####Network Functions


#####SNMPF Functions


#####URL Functions


#####Variable Functions


#####XML Functions


#####System Functions

- php_sapi_name() 返回 web 服务器和 PHP 之间的接口类型
- system() 输出并返回最后一行shell结果
- exec() 不输出结果，返回最后一行shell结果，所有结果可以保存到一个返回的数组里面
- passthru() 只调用命令，把命令的运行结果原样地直接输出到标准输出设备上

