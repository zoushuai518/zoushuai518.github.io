---
layout: post
title: "PHP设计模式之单例模式与静态变量"
category: php
comments: true
date:   2014-09-24 14:20:51
---

<span class="impor">干货：单例模式，貌似很吊的样子，说是可以降低PHP代码对内存的占用，其实确实也是如此。  
不过由于PHP本身设计的原因；导致其脚本的执行周期都是页面级别的。  
也就是说只是针对单次HTTP请求，且应用与不同场景的情况下有效，例如渲染一个页面的数据来源需要多次从数据库里获取，应该是吊丝程序员们最长用的场景</span>

#PHP静态变量

注：php中的变量（非Session）的最大作用域就是一次请求，每次请求都会重新初始化变量(包括 class中的 静态变量)  

在PHP中，静态变量的的存在意义仅仅是在某个结构体中（方法或者类）中传递一个变量，其作用域在此文件内。  
[在PHP中，没有普遍意义上的静态变量。与Java、C++不同，PHP中的静态变量的存活周期仅仅是每次PHP的会话周期，所以注定了不会有Java或者C++那种静态变量]  

demo1(面向过程:)  
{% highlight php linenos %}
<?php
function test(){
	static $var = 1;
	echo $var++.'<br />';
}
test();
test();
test();
//OutPut
//1
//2
//3
?>
{% endhighlight %}

从上面 test方法调用可以得出如下结论：  
在函数test的三次调用中，变量$var在三次调用中都是存在的，并且每次会递增1，而并没有清空或者重置  
所以可以得出一个结论，静态变量在当前结构体所在的生命周期中一直存在。当前的例子中，test函数的生命周期就是当前PHP脚本，只要程序没释放都是有效的  


demo2(面向对象:)  
{% highlight php linenos %}
<?php

class Test
{
	private static $_a = 1;
	private $_b = 2;
	public function add()
	{
		echo '$_a : ' . self::$_a++.'<br />';
		echo '$_b : ' . $this->_b++.'<br />';
	}
}
$class1 = new Test();
$class1->add();
$class1->add();
$class2 = new Test();
$class2->add();
$class2->add();
//Output
//$_a : 1
//$_b : 2
//$_a : 2
//$_b : 3
//$_a : 3
//$_b : 2
//$_a : 4
//$_b : 3

?>
{% endhighlight %}

从上面的类的运行结果来看，也得到了在函数中相同的结果  
那么大概总结一下就是说：  
<span class="impor">PHP的静态变量在所在对应的结构体的生命周期中永久存在，并且值保持一致，不论这个结构体被调用或者实例化了多少次。</span>
<span class="impor">其实这就是动态变量和静态变量的区别，</span>[具体看此篇文章](/php/2014/09/18/php%E5%8A%A8%E6%80%81%E5%8F%98%E9%87%8F%EF%BC%8C%E9%9D%99%E6%80%81%E5%8F%98%E9%87%8F%EF%BC%8C%E5%A0%86%E5%92%8C%E6%A0%88%E7%9A%84%E5%8C%BA%E5%88%AB)<span class="impor">。动态变量只在类中有效，而静态变量在当前php脚本。</span>


#PHP单例模式

- 问题：单例模式中静态变量到底有没有存在的必要。(zs:yes)
- 为什么要使用单例模式：  
&nbsp;&nbsp;&nbsp;&nbsp;这和PHP语言本身的设计有关系，可以说是PHP的一个缺点。 PHP语言是一种解释型的脚本语言，这种运行机制使得每个PHP页面被解释执行后，所有的相关资源都会被回收。也就是说，PHP在语言级别上没有办法让某个对象常驻内存，这和asp.net、Java等编译型是不同的，比如在Java中单例会一直存在于整个应用程序的生命周期里，变量是跨页面级的，真正可以做到这个实例在应用程序生命周期中的唯一性。然而在PHP中，所有的变量无论是全局变量还是类的静态成员，都是页面级的，每次页面被执行时，都会重新建立新的对象，都会在页面执行完毕后被清空，这样似乎PHP单例模式就没有什么意义了，<span class="impor">所以PHP单例模式我觉得只是针对单次页面级请求时出现多个应用场景并需要共享同一对象资源时是非常有意义的</span>。
- 单例模式在PHP中的应用场合：  
1.应用程序与数据库交互  
&nbsp;&nbsp;&nbsp;&nbsp;一个应用中会存在大量的数据库操作，比如过数据库句柄来连接数据库这一行为，使用单例模式可以避免大量的new操作，因为每一次new操作都会消耗内存资源和系统资源  
2.控制全局配置信息  
&nbsp;&nbsp;&nbsp;&nbsp;如果系统中需要有一个类来全局控制某些配置信息, 那么使用单例模式可以很方便的实现.  
3.日志写入对象  
&nbsp;&nbsp;&nbsp;&nbsp;如果系统中需要添加日志，用来分析程序的行为和方便问题的查找，可以用单例模式实现.  

##理解1：
- 使用场景：PHP单例模式一般使用在,链接数据库的DB类(仅作为参考 | 具体，使用场景和设计方法，还需要在查资料)
- 优点：单例模式可以避免大量的new操作，因为每一次new操作都会消耗内存资源和系统资源
- 缺点：在PHP中，所有的变量无论是全局变量还是类的静态成员，都是页面级的，每次页面被执行时，都会重新建立新的对象，都会在页面执行完毕后被清空，这样似乎PHP单例模式就没有什么意义了，所以PHP单例模式我觉得只是针对单次页面级请求时出现多个应用场景并需要共享同一对象资源时是非常有意义的


demo3:  
{% highlight php linenos %}
<?php

class A
{
	private static $_instance = null;
	private $_b = 1;

	/**
	 * 构造方法 属性设置为私有,这样可以防止在class外部用 new来实例化对象
	 */
	private function __construct(){
		
	}
	public static function getInstance()
	{
		if(self::$_instance == null){
			$classname = __CLASS__;
			self::$_instance = new $classname();
			//return new $classname();	// 输出2,1 也就是说单例模式必须拥有一个保存类的实例的静态成员变量
		}
		return self::$_instance;
	}

	public function add()
	{
		$this->_b++;
	}

	public function show()
	{
		echo $this->_b;
	}
}

//Output
$a = A::getInstance();
$b = A::getInstance();
var_dump($a);
var_dump(A::$_instance);
var_dump($b);
var_dump(A::$_instance);
//此处$a和$b 变量完全相同！
$a->add();
$a->show();
echo '<br />';
$b->show();
//output
//2
//2

?>
{% endhighlight %}

- 上例中，由于单例模式存在，使得$a和$b完全是同一个对象，<span class="impor">所以之间如果需要共享数据，完全不需要静态变量</span>（废话，就是自己。因为在任何时候，应用程序中都只会有这个类仅有的一个实例存在！无论你调用多少次单例，里面的数据是不会被重新实例化的。）  

- 也就是说，在单例模式中，静态变量根本就没有存在的意义。当然，如果你没事干，非要使用new方法来初始化对象的话，也行，此时单例模式被打破，回归到无单例模式的状态  

- 备注：如果为了防止使用new来实例化对象，那么可以考虑对类的__construct函数设置为private属性(见代码)  
{% highlight php linenos %}
<?php
//如果尝试用new来实例化的话
$c = new A();
//output
//Fatal error: Call to private A::__construct() from invalid context in
//如果需要A类的实例化对象，只能通过开放的get_instance静态方法进行初始化
?>
{% endhighlight %}


##理解2(使用此)：

<span class="impor">单例模式的三个特点：</span>  

1. 一个类只能有一个实例
2. 它必须自行创建这个实例
3. 它必须自行向整个系统提供这个实例


<span class="impor">单例模式的三个要点：</span>  

- 拥有一个保存类的实例的静态成员变量
- 构造函数需要标记为private（访问控制：防止外部代码使用new操作符创建对象），单例类不能在其他类中实例化，只能被其自身实例化；
- 拥有一个访问这个实例的公共的静态方法（常用getInstance()方法进行实例化单例类，通过instanceof操作符可以检测到类是否已经被实例化）|(为该唯一单例提供一个全局访问点(一般是一个静态的getInstance方法))
- 另外，需要创建__clone()方法防止对象被复制（克隆）


为什么要使用PHP单例模式？  

- php的应用主要在于数据库应用, 所以一个应用中会存在大量的数据库操作, 使用单例模式, 则可以避免大量的new 操作消耗的资源。
- 如果系统中需要有一个类来全局控制某些配置信息, 那么使用单例模式可以很方便的实现. 这个可以参看ZF的FrontController部分。
- 在一次页面请求中, 便于进行调试, 因为所有的代码(例如数据库操作类db)都集中在一个类中, 我们可以在类中设置钩子, 输出日志，从而避免到处var_dump, echo。

{% highlight php linenos %}
<?php

/**
 * 设计模式之单例模式
 * $_instance必须声明为静态的私有变量
 * 构造函数和析构函数必须声明为私有,防止外部程序new
 * 类从而失去单例模式的意义
 * getInstance()方法必须设置为公有的,必须调用此方法
 * 以返回实例的一个引用
 * ::操作符只能访问静态变量和静态函数
 * new对象都会消耗内存
 * 使用场景:最常用的地方是数据库连接。 
 * 使用单例模式生成一个对象后，
 * 该对象可以被其它众多对象所使用。 
 */
class Danli {
 
	//保存类实例的静态成员变量
	private static $_instance;
  
	//private标记的构造方法
	private function __construct(){
		echo 'This is a Constructed method;';
	}
   
	//创建__clone方法防止对象被复制克隆
	public function __clone(){
		trigger_error('Clone is not allow!',E_USER_ERROR);
	}
    
	//单例方法,用于访问实例的公共的静态方法
	public static function getInstance(){
		// instanceof 关键字
		if(!(self::$_instance instanceof self)){
			self::$_instance = new self;
		}
		return self::$_instance;
	}
	 
	public function test(){
	   echo '调用方法成功';
	}
	  
}
	   
	//用new实例化private标记构造函数的类会报错
	//$danli = new Danli();

	//正确方法,用双冒号::操作符访问静态方法获取实例
	$danli = Danli::getInstance();
	$danli->test();

	//复制(克隆)对象将导致一个E_USER_ERROR
	$danli_clone = clone $danli;

?>
{% endhighlight %}

[php单例模式参考](http://blog.samoay.me/post/view/13)  

