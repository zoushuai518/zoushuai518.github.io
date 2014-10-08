---
layout: post
title: "获得PHP代码占用内存的情况"
category: php
comments: true
date:   2014-09-12 10:59:51
---

# 获得PHP代码占用内存的情况

想要知道编写的 PHP 脚本需要占用多少内存么？很简单，直接使用 PHP 查看当前分配给 PHP 脚本的内存的函数 memory_get_usage() 就可以了。  

demo1:  
{% highlight php linenos %}
<?php
echo memory_get_usage(), '<br />';
$tmp = str_repeat('http://www.nowamagic.net/', 4000);
echo memory_get_usage(), '<br />';
unset($tmp);
echo memory_get_usage();
?>
{% endhighlight %}

上面的程序后面的注释代表了它们的输出（单位为 byte(s)），也就是当时 PHP 脚本使用的内存（不含 memory_get_usage() 函数本身占用的内存）。
由上面的例子可以看出，要想减少内存的占用，可以使用 PHP unset() 函数把不再需要使用的变量删除。类似的还有：PHP mysql_free_result() 函数，可以清空不再需要的查询数据库得到的结果集，这样也能得到更多可用内存。
PHP memory_get_usage() 函数还可以有个参数，$real_usage，其值为布尔值。默认为 FALSE，表示得到的内存使用量不包括该函数（PHP 内存管理器）占用的内存；当设置为 TRUE 时，得到的内存为包括该函数（PHP 内存管理器）占用的内存。
所以在实际编程中，可以用 memory_get_usage() 函数比较各个方法占用内存的高低，来选择使用哪种占用内存小的方法。  

demo2:  
{% highlight php linenos %}
<?php

/**
 * PHP单位转换
 * @param int $size 大小,单位b
 * @return 返回单位大小
 */
function formatBytes($size) { 
  $units = array(' B', ' KB', ' MB', ' GB', ' TB'); 
  for ($i = 0; $size >= 1024 && $i < 4; $i++) $size /= 1024; 
  return round($size, 2).$units[$i]; 
 }

/**
 * 获取程序占用内存情况
 * @param boolean $real_usage FALSE不包括函数本身占用内存,TRUE包括函数本身占用内容
 * @return int $memoryUsage 返回php代码占用的内存
 */
function get_memory_usage($real_usage = FALSE)
{
	if (!function_exists('memory_get_usage')) 
	{
		/**
		 * 取得内存使用情况
		 * @return integer
		 */
		//function memory_get_usage()
		//{
			$pid = getmypid();
			if (IS_WIN) 
			{
				exec('tasklist /FI "PID eq ' . $pid . '" /FO LIST', $output);
				return preg_replace('/[^0-9]/', '', $output[5]) * 1024;
			} 
			else 
			{
				exec("ps -eo%mem,rss,pid | grep $pid", $output);
				$output = explode(" ", $output[0]);
				return $output[1] * 1024;
			}
		//}
	} else {
		return memory_get_usage($real_usage);
		//return formatBytes(memory_get_usage($real_usage));
	}
}

//memory_get_usage();  
$m1 = get_memory_usage();  
//$m1 = memory_get_usage();  
echo '<br /> m1:',formatBytes($m1);
  
$a = 'hello';  
$b =  str_repeat($a,1000);  
  
$m2 = memory_get_usage();  
echo '<br /> m2:',formatBytes($m2);
  
unset($b);  
  
$m3 = memory_get_usage();  
echo '<br /> m3:',formatBytes($m3);

?>
{% endhighlight %}

[原文链接](http://www.nowamagic.net/php/php_MemoryGetUsage.php)  

