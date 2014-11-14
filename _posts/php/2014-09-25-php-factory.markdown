---
layout: post
title: "PHP设计模式之工厂模式(Factory Pattern)"
category: php
comments: true
date:   2014-09-25 10:50:51
---

#工厂模式概述：

<span class="impor">干货：工厂模式，就是用于动态创建对象,返回对象的实例；这种设计,低耦合、高扩展；遵循了软件设计原则的  对扩展开发，对修改封闭</span>

###1. 定义：定义一个用于创建对象的接口，让子类决定实例化哪一个类，工厂方法使一个类的实例化延迟到其子类。  

###2. 工厂模式：首先需要说一下工厂模式。工厂模式根据抽象程度的不同分为三种：简单工厂模式（也叫静态工厂模式）、本文所讲述的工厂方法模式、以及抽象工厂模式。工厂模式是编程中经常用到的一种模式。  

它的主要优点有:

- 可以使代码结构清晰，有效地封装变化。在编程中，产品类的实例化有时候是比较复杂和多变的，通过工厂模式，将产品的实例化封装起来，使得调用者根本无需关心产品的实例化过程，只需依赖工厂即可得到自己想要的产品。

- 对调用者屏蔽具体的产品类。如果使用工厂模式，调用者只关心产品的接口就可以了，至于具体的实现，调用者根本无需关心。即使变更了具体的实现，对调用者来说没有任何影响。

- 降低耦合度。产品类的实例化通常来说是很复杂的，它需要依赖很多的类，而这些类对于调用者来说根本无需知道，如果使用了工厂方法，我们需要做的仅仅是实例化好产品类，然后交给调用者使用。对调用者来说，产品所依赖的类都是透明的。
  

###3. 工厂方法模式(抽象工厂模式)：  
<span class="impor">工厂方法模式有四个要素：</span>

- 工厂接口。工厂接口是工厂方法模式的核心，与调用者直接交互用来提供产品。在实际编程中，有时候也会使用一个抽象类来作为与调用者交互的接口，其本质上是一样的。

- 工厂实现。在编程中，工厂实现决定如何实例化产品，是实现扩展的途径，需要有多少种产品，就需要有多少个具体的工厂实现。

- 产品接口。产品接口的主要目的是定义产品的规范，所有的产品实现都必须遵循产品接口定义的规范。产品接口是调用者最为关心的，产品接口定义的优劣直接决定了调用者代码的稳定性。同样，产品接口也可以用抽象类来代替，但要注意最好不要违反里<span class="impor">氏替换原则。</span>

- 产品实现。实现产品接口的具体类，决定了产品在客户端中的具体行为。

前文提到的简单工厂模式跟工厂方法模式极为相似，区别是：<span class="impor">简单工厂只有三个要素，他没有工厂接口，并且得到产品的方法一般是静态的。因为没有工厂接口，所以在工厂实现的扩展性方面稍弱</span>。可以算所工厂方法模式的简化版，关于简单工厂模式，在此一笔带过。  

###4. 适用场景：
- 不管是简单工厂模式，工厂方法模式还是抽象工厂模式，他们具有类似的特性，所以他们的适用场景也是类似的。
- 首先，作为一种创建类模式，在任何需要生成复杂对象的地方，都可以使用工厂方法模式。有一点需要注意的地方就是复杂对象适合使用工厂模式，而简单对象，特别是只需要通过new就可以完成创建的对象，无需使用工厂模式。如果使用工厂模式，就需要引入一个工厂类，会增加系统的复杂度。
- 其次，<span class="impor">工厂模式是一种典型的解耦模式</span>，<span class="impor">迪米特法则</span>在工厂模式中表现的尤为明显。假如调用者自己组装产品需要增加依赖关系时，可以考虑使用工厂模式。将会大大降低对象之间的耦合度。
- 再次，由于工厂模式是依靠抽象架构的，它把实例化产品的任务交由实现类完成，<span class="impor">扩展性比较好</span>。也就是说，当需要系统有比较好的扩展性时，可以考虑工厂模式，不同的产品用不同的实现工厂来组装。

###5. 低耦合、高扩展：


#工厂模式的分类：

- 简单工厂模式(Simple Factory)
- 工厂方法模式(Factory Method)
- 抽象工厂模式(Abstract Factory)

这三种模式从上到下逐步抽象，并且更具一般性。  

简单工厂模式又称静态工厂方法模式。重命名上就可以看出这个模式一定很简单。它存在的目的很简单：定义一个用于创建对象的接口。工厂方法模式去掉了简单工厂模式中工厂方法的静态属性，使得它可以被子类继承。这样在简单工厂模式里集中在工厂方法上的压力可以由工厂方法模式里不同的工厂子类来分担。  

工厂方法模式仿佛已经很完美的对对象的创建进行了包装，使得客户程序中仅仅处理抽象产品角色提供的接口。那我们是否一定要在代码中遍布工厂呢？大可不必。也许在下面情况下你可以考虑使用工厂方法模式：  

- 当客户程序不需要知道要使用对象的创建过程。
- 客户程序使用的对象存在变动的可能，或者根本就不知道使用哪一个具体的对象。


#典型应用DEMO1：

###1. 简单工厂(静态工厂方法)
先看一下“简单工厂”，简单工厂实际上还算不上一种模式，我们可以把它叫做一种常用写法。例如我们比较常用的数据库操作，我们为了适配不同的数据库引擎，可能分别编写了具体的数据库操作类（当然这些类都实现了统一的接口）：  
{% highlight php linenos %}
<?php

//统一的操作接口，保证对于外部调用是透明统一的
interface DbInterface{
    public function connect(Array $params=array());
    public function query($sql);
    public function insert($table, $record);
    public function update($table, $record, $where);
    public function delete($table, $where);
}

class Mysql implements DbInterface{
    //...省略具体实现
}

class SqlServer implements DbInterface{
    //...省略具体实现
}

class Sqlite implements DbInterface{
    //...省略具体实现
}

//我们具体的应用中，一般只会使用一种数据引擎，一般会根据一个全局的配置
class GlobalConfig{
    const DBENGINE = "mysql";
}

//原始的方法每次需要使用数据库的时候都使用if或者switch方法判断配置并实例化需要的数据库操作类。
//这样代码中会出现大量重复的代码，那么我们应该将相应的代码迁移到一个统一的类当中，这就是简单工厂了
class DbFactory{
    //一般为了调用便利，不需要初始化这个工具类，将方法定义为静态方法
    public static function factory(){
        if (GlobalConfig::DBENGINE=='mysql'){
            return new Mysql();
        }elseif (GlobalConfig::DBENGINE=='sqlserver'){
            return new SqlServer();
        }elseif (GlobalConfig::DBENGINE=='sqlite'){
            return new Sqlite();
        }
    }
}

//需要使用时
$db = DbFactory::factory();
$db->connect(...);//当然，我们可以把connect操作在类的构造方法中去实现了

?>
{% endhighlight %}
简单工厂只是工厂方法模式抽象中的一个部分，但是因为其结构简单而又实用（将复杂的初始化操作封装起来了），在PHP中应用广泛，Zend Framework中随处可见，如Cache、Captcha、Ldap等等。  

###2. 工厂方法模式(抽象工厂模式)
工厂方法模式，是指通过实现一个类似现实中“工厂”的模式，用来生产我们想要的产品即具体类的实例。通过工厂方法模式，我们先定义一个创建对象的接口（抽象工厂），然后将具体类（产品）的实例化推迟到了子类（具体工厂）中实现。那么在我们项目中如何具体应用呢？

假如我们的产品需要提供数据导出的功能，目前我们提供两种格式的导出：txt文本和pdf文档。安装工厂方法模式，我们这样来实现：  
{% highlight php linenos %}
<?php

//抽象工厂
// <?php
interface ExportFactory{
    //这里的config我们用于传入导出的一些需要的特殊配置，与默认配置进行合并
    //如果是自己的项目框架内使用，除了通过方法传入参数，还可以通过配置文件的方式
    public function create($config=array());
}

// <?php
//Txt格式具体工厂
class TxtExportFactory implements ExportFactory{
    public function create($config=array()){
        return new TxtExportHelper($config);
    }
}

// <?php
//Pdf格式具体工厂
class PdfExportFactory implements ExportFactory{
    public function create($config=array()){
        return new PdfExportHelper($config);
    }
}

// <?php
//抽象产品
interface ExportHelper{
    /**
     * @param $source 导出数据源
     */
    public function export($source);
}

// <?php
//具体Txt导出类
class TxtExportHelper implements ExportHelper{
    private $config;

    public function __construct($config==array()){
        $defaultConfig = array(
            'maxsize' => 1024*1024*5,
            'rootdir' => './export/',
            'prefix' => 'ex_',
            'coding' => 'UTF-8',
            'linechars' => 100,
        );
        $this->config = array_merge($defaultConfig, $config);
    }

    public function export($source){
        //具体导出逻辑，这里省略
    }
}

// <?php
//具体Pdf导出类
class PdfExportHelper implements ExportHelper{
    private $config;

    public function __construct($config=array()){
        $defaultConfig = array(
            'maxsize' => 1024*1024*5,
            'rootdir' => './export/',
            'prefix' => 'ex_',
            'width' => 1800,
            'height' => 3200,
            'margin' => 200,
        );
        $this->config = array_merge($defaultConfig, $config);
    }

    public function export($source){
        //具体导出逻辑，这里省略
    }
}

// <?php
//Client调用，假定我们有一个博客文章阅读的Controller（MVC）如下：
class NovelController{
    public function actionView($id){
        //html页面显示$id内容
    }

    public function actionVote($id){
        //给$id内容投票
    }

    public function actionExportTxt($id){
        //导出$id的内容
        $source = $this->modle->getDataByPk($id);//假定调用Model提取数据
        $factory = new TxtExportFactory();
        $this->export($factory);//txt方式完全使用默认配置
    }

    public function actionExportPdf($id){
        //导出$id的内容
        $source = $this->modle->getDataByPk($id);//假定调用Model提取数据
        $config = array(
            'width' => 1200,
            'height' => 1800,
        );
        $factory = new PdfExportFactory();
        $this->export($factory, $source, $config);
    }

    //将具体的导出处理独立
    private function export(ExportFactory $factory, $source, $config=array()){
        $exportHelper = $factory->create($config);
        $exportHelper->export($source); 
    }

    //省略更多方法
}
?>
{% endhighlight %}


#关于“工厂方法模式”相对于“简单工厂”的优势

<span class="impor">实际上，上诉“工厂方法模式”完全可以通过“简单工厂”去实现，那么我们为什么要抽象出“抽象工厂”和“具体工厂”这两个角色呢？很简单，还是为了更好的实现封装，说到这里不得不说软件设计中的一条重要原则：  
开放-封闭原则。就是说，我们的设计应该只对扩展开放（可以通过扩展现有结构实现新的功能），而应该对修改封闭（不应该修改已有的类和设计）。</span>

那么针对我们封装的这个功能：实现数据的多文件格式导出，如果使用“简单工厂”去实现去实现，例如`ExportFactory`如下：  
{% highlight php linenos %}
<?php
class ExportFactory{
    pubic static function create($type){
        switch ($type){
            case 'txt':
                return new TxtExportHelper();
            case 'pdf':
                return new PdfExportHelper();
            default:
                throw new Exception('export type not suppert');
        }
    }
}
?>
{% endhighlight %}

如果我们现在业务扩展，需要实现新的格式Word文档的导出，那么，我们不得不修改上述ExportFactory类，增加一条case语句：  
{% highlight php linenos %}
<?php
// ...
case 'word':
    return new WordExportHelper();
// ...
?>
{% endhighlight %}

这就表示，我们修改了已有的类`ExportFactory`。随着业务的不断扩展，我们可能还需要经常修改这个类，来增加诸如Epub格式、mobi格式等等的支持。而且，当case分支越来越多，这个`ExportFactory`将变得更加难于维护。那如果使用“工厂方法模式”来实现呢，我们就只需要新增`WordExportFactory`和`WordExportHelper`这两个类，客户端（Client）就可以直接使用了。

可能有的人会想，那我新增了两个类不算“修改”吗？我在客户端`NovelController`中增加对Word格式处理的调用不算修改吗？对于前者，在软件设计中不能称为修改（没有修改已经有的类和设计结构），而是扩展（通过实现`ExportFactory`和`ExportHelper`接口扩展已有的功能）。对于后者，的确是修改，但是这种修改是延迟到了客户端（调用者）的，所以对于我们定义的功能“实现数据的多文件格式导出”并没有修改，而这种延迟对于底层设计者和最终调用者都是有好处的。


#典型应用DEMO2：

- 【主要角色】
抽象产品(Product)角色：具体产品对象共有的父类或接口  
具体产品(Concrete Product)角色：实现抽象产品角色所定义的接口，并且工厂方法模式所创建的每一个对象都是某具体产品对象的实例  
抽象工厂(Creator)角色：模式中任何创建对象的工厂类都要实现这个接口，它声明了工厂方法，该方法返回一个Product类型的对象。  
Creator也可以定义一个工厂方法的缺省实现，它返回一个缺省的的ConcreteProduct对象  
具体工厂(Concrete Creator)角色：实现抽象工厂接口，具体工厂角色与应用逻辑相关，由应用程序直接调用以创建产品对象。  

- 【优缺点】
优点：工厂方法模式可以允许系统在不修改工厂角色的情况下引进新产品。  
缺点：客户可能仅仅为了创建一个特定的ConcreteProduct对象，就不得不创建一个Creator子类  

- 【适用性】  
1、当一个类不知道它所必须创建的对象的类的时候  
2、当一个类希望由它的子类来指定它所创建的对象的时候  
3、当类将创建对象的职责委托给多个帮助子类的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候  
{% highlight php linenos %}
<?php

//工厂方法模式
// 垃圾!!!

 /** 
 * 工厂方法模式 
 * ------------- 
 * @author      zhaoxuejie <zxj198468@gmail.com> 
 * @package     design pattern  
 * @version     v1.0 2011-12-14 
 */  
  
//抽象产品  
interface Work {  
    public function doWork();   
}  
  
//具体产品实现  
class Student implements Work {  
      
    function doWork(){  
        echo "学生做作业！\n";  
    }  
}  
  
class Teacher implements Work {  
      
    function doWork(){  
        echo "老师批改作业！\n";  
    }  
}  
  
//抽象工厂  
interface WorkerFactory {  
    public function getWorker();  
}  
  
//具体抽象工厂实现  
class StudentFactory {  
      
    function getWorker(){  
        return new Student();  
    }  
}  
  
class TeacherFactory {  
    function getWorker(){  
        return new Teacher();  
    }  
}  
  
//客户端  
class Client {  
      
    static function main() {  
        $s = new Student();  
        $s->doWork();  
          
        $t = new Teacher();  
        $t->doWork();  
    }  
}  
  
Client::main();  

?>
{% endhighlight %}

{% highlight php linenos %}
<?php
/*【简单工厂模式】
从设计模式的类型上来说，简单工厂模式是属于创建型模式，又叫做静态工厂方法（StaticFactory Method）模式，但不属于23种GOF设计模式之一。简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是工厂模式家族中最简单实用的模式，可以理解为是不同工厂模式的一个特殊实现。
*/
 /** 
 * 简单工厂模式 
 * ------------- 
 * @author      zhaoxuejie <zxj198468@gmail.com> 
 * @package     design pattern  
 * @version     v1.0 2011-12-14 
 */  
  
interface  Comput {  
    public function getResults();     
}  
  
//操作类  
class Operation {  
    protected  $Number_A = 0;  
    protected  $Number_B = 0;  
    protected  $Result = 0;  
      
    //赋值  
    function setNumber($Number_A, $Number_B){  
        $this->Number_A = $Number_A;  
        $this->Number_B = $Number_B;  
    }  
      
    //清零  
    function clearResult(){  
        $this->Result = 0;  
    }  
}  
  
//加法  
class OperationAdd extends Operation implements Comput {  
    function getResults(){  
        return $this->Result = ($this->Number_A + $this->Number_B);      
    }  
}  
  
//减法  
class OperationSub extends Operation implements Comput {  
    function getResults(){  
        return $this->Result = ($this->Number_A - $this->Number_B);      
    }  
}  
  
//乘法  
class OperationMul extends Operation implements Comput {  
    function getResults(){  
        return $this->Result = ($this->Number_A * $this->Number_B);      
    }  
}  
  
//除法  
class OperationDiv extends Operation implements Comput {  
    function getResults(){  
        if(intval($this->Number_B) == 0){  
            return $this->Result = 'Error: Division by zero';  
        }  
        return $this->Result = ($this->Number_A / $this->Number_B);      
    }  
}  
  
//工厂  
class OperationFactory {  
    private static $obj;  
      
    public static function CreateOperation($type){  
        try {  
           $error = "Please input the '+', '-', '*', '/' symbols of Math.";  
           switch($type){  
                case '+' :  
                    self::$obj = new OperationAdd();  
                    break;  
                case '-' :  
                    self::$obj = new OperationSub();  
                    break;  
                case '*' :  
                    self::$obj = new OperationMul();  
                    break;  
                case '/' :  
                    self::$obj = new OperationDiv();  
                    break;  
                default:  
                    throw new Exception($error);  
            }  
            return self::$obj;  
          
        } catch (Exception $e) {  
            echo 'Caught exception: ',  $e->getMessage(), "\n";  
            exit;  
        }  
    }  
}  
  
//工厂创建实例  
$obj = OperationFactory::CreateOperation('*');  
$obj->setNumber(3, 4);  
echo $obj->getResults();  
?>
{% endhighlight %}

[理解PHP的工厂模式Factory Pattern](http://www.nowamagic.net/librarys/veda/detail/377)  
[PHP设计模式之工厂方法模式(Factory Method)](http://blog.samoay.me/post/view/27)  
[php实现工厂模式](http://blog.csdn.net/zhaoxuejie/article/details/7072878)

