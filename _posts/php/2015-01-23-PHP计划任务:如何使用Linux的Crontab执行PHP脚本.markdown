---
layout: post
title: "PHP计划任务:如何使用Linux的Crontab执行PHP脚本"
category: php
comments: true
date:   2015-01-23 14:20:51
---

我们的PHP程序有时候需要定时执行，我们可以使用ignore_user_abort函数或是在页面放置js让用户帮我们实现。但这两种方法都不太可靠，不稳定。我们可以借助Linux的Crontab工具来稳定可靠地触发PHP执行任务。
下面介绍Crontab的两种方法。


一、在Crontab中使用PHP执行脚本  
就像在Crontab中调用普通的shell脚本一样（具体Crontab用法），使用PHP程序来调用PHP脚本  
每一小时执行myscript.php如下:  
shell> crontab -e  
00 * * * * /usr/local/bin/php /home/john/myscript.php  
/usr/local/bin/php为PHP程序的路径。


二、在Crontab中使用URL执行脚本  
如果你的PHP脚本可以通过URL触发，你可以使用lynx或curl或wget来配置你的Crontab。  
下面的例子是使用Lynx文本浏览器访问URL来每小时执行PHP脚本。Lynx文本浏览器默认使用对话方式打开URL。  

但是，像下面的，我们在lynx命令行中使用-dump选项来把URL的输出转换来标准输出。  
00 * * * * lynx -dump http://www.centos.bz/myscript.php  

下面的例子是使用CURL访问URL来每5分执行PHP脚本。Curl默认在标准输出显示输出。使用"curl -o"选项，你也可以把脚本的输出转储到临时文件。  
*/5 * * * * /usr/bin/curl -o temp.txt http://www.centos.bz/myscript.php  

下面的例子是使用WGET访问URL来每10分执行PHP脚本。-q选项表示安静模式。"-O temp.txt"表示输出会发送到临时文件。    
*/10 * * * * /usr/bin/wget -q -O temp.txt http://www.centos.bz/myscript.php