---
layout: post
title: "PHP instanceof运算符"
category: php
comments: true
date:   2014-09-18 14:20:51
---

#一.简单的示例：

概念：instanceof函数是php5中新添的面向对象的函数。他主要的作用是 检测一个给定的对象是否属于（继承于）某个类（class）、某个类的子类、某个接口（interface）。如果是则返回true。反之返回false;

理解demo1：
{% highlight php linenos %}
<?php

class baseClass {
	
} 

class subClass extends baseClass{
	
}  

interface aInterface {
	
} 

class aClass implements aInterface {
	
}

$a = new baseClass();
var_dump( ($a instanceof baseClass));
//true

$b = new subClass();
var_dump( ($b instanceof baseClass));
//true

$c = new aClass();
var_dump( ($c instanceof aInterface));
//true
var_dump( ($c instanceof subClass)); 
//false

?>
{% endhighlight %}


#二.再理解：
在PHP5中，通过方法传递变量的类型有不确定性。于是我们很难判断，一些操作是否可以运行。  
使用instanceof运算符，可以判断当前实例是否可以有这样的一个形态。当前实例使用 instanceof与当前类，父类（向上无限追溯），已经实现的接口比较时，返回真。  
`代码格式：实例名 instanceof 类名`

淡淡的忧伤：  
- 对于PHP面向对象，类中的方法的参数可以是对象，在传对象的过程中任何类的对象都可以传进去，但是不同的对象传入到此方法后效果不同，比如此方法用一个对象可以调用这个对象的方法，但是如果传入的对象没有此方法，那就会报错，因为根本就没有你调用的方法啊，所以传入的对象是要有判断的。
- 曾经我们讲过一个解决方法，那就是把方法的参数类型确定，对象参数也要指定对象类型即可，但是今天讲的是instanceof运算符来保障代码正常运行，用instanceof运算符可以在方法里面判断，性能更好，下面是一个运用instanceof运算符的例子和没用时的比较。

1.正常运行的代码：
{% highlight php linenos %}
<?php
classUser{
	private $name;
	public function  getName(){
		return "UserName is ".$this->name;
	}
}

class NormalUser extends User {
	private $age = 99;
	public function getAge(){
		return "age is ".$this->age;
	}
}

class UserAdmin{ //操作.
	public static function  getUserInfo(User $_user){
		echo $_user->getAge();
	}
}

$normalUser = new NormalUser();
UserAdmin::getUserInfo($normalUser);

//程序运行结果：
//age is 99

?>
{% endhighlight %}

2.运行异常的代码：
{% highlight php linenos %}
<?php

class User{
	private $name;
	public function  getName(){
		return "UserName is ".$this->name;
	}
}

class NormalUser extends User {
	private $age = 99;
	public function getAge(){
		return "age is ".$this->age;
	}
}

class UserAdmin{ //操作.
	public static function  getUserInfo(User $_user){
		echo $_user->getAge();
	}
}

$User = new User(); // 这里new的是User.
UserAdmin::getUserInfo($User);

//程序运行结果：
//Fatal error:  Call to undefined method User::getAge() in E:\PHPProjects\NowaMagic\php\php_InstanceofOperator.php on line 99

?>
{% endhighlight %}

3.使用instatnceof运算符保障代码安全，阻止抛出异常：
{% highlight php linenos %}
<?php
//使用instatnceof运算符，在操作前先进行类型判断。以保障代码的安全性
class User{
	private $name;
	public function  getName(){
		return "UserName is ".$this->name;
	}
}

class NormalUser extends User {
	private $age = 99;
	public function getAge(){
		return "age is ".$this->age;
	}
}

class UserAdmin{ //操作.
	public static function  getUserInfo(User $_user){
		if($_user instanceof NormalUser ){
			echo $_user->getAge();
		}else{
			echo "类型不对,不能使用这个方法.";
		}
	}
}

$User = new User(); // 这里new的是User.
UserAdmin::getUserInfo($User);

//程序运行结果：
//类型不对,不能使用这个方法.

?>
{% endhighlight %}

