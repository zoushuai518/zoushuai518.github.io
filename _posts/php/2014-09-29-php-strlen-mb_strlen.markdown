---
layout: post
title: "php strlen和mb_strlen的区别"
category: php
comments: true
date:   2014-09-29 14:41:51
---

<span class="impor">干货：strlen和mb_strlen都是返回字符串的长度；strlen纯英文ok；纯中文、中英混合两个返回长度不同；后者建议使用mb_strlen。  
mb_strlen不是php内置函数，使用此函数，需要开启php_mbstring扩展。
</span>


- strlen返回的是字符串的</span class="impor">字节数</span>。(英文字符串长度和字节数相同)
- mb_strlen真正返回的是字符串的长度，它和字符编码相关。(gbk:{英文：1个字符占用1个字节长度；中文：1个字符占用2个字节长度}；utf8:{英文：1个字符占用1个字节长度；中文：1个字符占用3个字节长度})

- strlen不能处理中文或者中英文混合字符串
- mb_strlen可以处理，strlen所不能的字符串

###DEMO:
{% highlight php linenos %}
<?php

//测试时文件的编码方式要是UTF8  
$str='中文a字1符';  
echo strlen($str).'<br>';//14  
echo mb_strlen($str,'utf8').'<br>';//6  
echo mb_strlen($str,'gbk').'<br>';//8  
echo mb_strlen($str,'gb2312').'<br>';//10  

//结果分析：在strlen计算时，对待一个UTF8的中文字符是3个长度，所以“中文a字1符”长度是3*4+2=14
//在mb_strlen计算时，选定内码为UTF8，则会将一个中文字符当作长度1来计算，所以“中文a字1符”长度是6 .

?>
{% endhighlight %}


- [strlen](http://cn2.php.net/strlen)
- [mb_strlen](http://cn2.php.net/manual/zh/function.mb-strlen.php)
- [mb_internal_encoding](http://cn2.php.net/manual/zh/function.mb-internal-encoding.php)
