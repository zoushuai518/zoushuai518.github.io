---
layout: post
title: "php composer"
category: php
comments: true
date:   2014-09-12 10:59:51
---

# php 包管理之 composer

***简介：***
对于现代语言而言，包管理器基本上是标配。Java有Maven，Python有pip，Ruby有gem，Nodejs有npm。PHP的则是[PEAR](http://pear.php.net/)  


#一.前言：  
1.php PEAR包管理有很多问题  

*   依赖处理容易出问题  
*   配置复杂.  
*   命令行接口难用.  

2.composer，是新出的php依赖管理工具  

*   开源.  
*   使用简单.  
*   可以简单的提交自己的开发包.  


#二.安装composer:  
Composer需要php 5.3.2+才能运行  
<pre>
<span class="Variable">$</span>curl -sS <span class="symbol">https:</span>/<span class="regex">/getcomposer.org/installer</span> | php
</pre>
这个命令会将<code>composer.phar</code>下载到当前目录。PHAR（PHP 压缩包）是一个压缩格式，可以在命令行下直接运行  
你可以使用<code>--install-dir</code>选项将Composer安装到指定的目录，例如：  
<pre>
<span class="Variable">$</span>curl -sS<span class="symbol"> https:</span>/<span class="regex">/getcomposer.org/installer</span> | php -- --install-dir=bin
</pre>
当然也可以进行全局安装：  
<pre>
<span class="Variable">$</span>curl -sS<span class="symbol"> https:</span>/<span class="regex">/getcomposer.org/installer</span> | php
<span class="Variable">$</span>mv composer.phar /usr/local/bin/composer
</pre>
不过通常情况下只需将`composer.phar`的位置加入到`PATH`就可以，不一定要全局安装


#三.声明依赖  
在项目目录下创建一个composer.json文件，指明依赖，比如，你的项目依赖 [monolog：](https://github.com/Seldaek/monolog)  
``
{
	"require": {
		"monolog/monolog": "1.2.*"
	}
}
``

#四.安装依赖
安装依赖非常简单，只需在项目目录下运行：  
`composer install`

如果没有全局安装的话，则运行：  
`php composer.phar install`


#五.自动加载
Composer提供了自动加载的特性，只需在你的代码的初始化部分中加入下面一行：  
`require 'vendor/autoload.php';`


#六.模块仓库
[packagist.org](https://packagist.org/)是Composer的仓库，很多著名的PHP库都能在其中找到。你也可以[提交你自己的作品](https://packagist.org/packages/submit)。


#七.高级特性(5个Composer小技巧)
Composer是新一代的PHP依赖管理工具  


###1. 仅更新单个库
只想更新某个特定的库，不想更新它的所有依赖，很简单：  
<pre>
composer update foo/bar
</pre>

此外，这个技巧还可以用来解决“警告信息问题”。你一定见过这样的警告信息：  
<pre>
Warning: The lock file is not up to date with the latest changes in composer.json, you may be getting outdated dependencies, run update to update them.
</pre>

擦，哪里出问题了？别惊慌！如果你编辑了<code>composer.json</code>，你应该会看到这样的信息。比如，如果你增加或更新了细节信息，比如库的描述、作者、更多参数，甚至仅仅增加了一个空格，都会改变文件的md5sum。然后<code>Composer</code>就会警告你哈希值和<code>composer.lock</code>中记载的不同。  

那么我们该怎么办呢？<code>update</code>命令可以更新lock文件，但是如果仅仅增加了一些描述，应该是不打算更新任何库。这种情况下，只需<code>update nothing</code>：  
<pre>$ composer update nothing<br />
Loading composer repositories <span class="string">with package information</span><br />
<span class="red">Updating dependencies<br />
Nothing to install or update<br />
Writing lock file<br />
Generating autoload files</span>
</pre>

这样一来，<code>Composer</code>不会更新库，但是会更新<code>composer.lock</code>。注意<code>nothing</code>并不是<code>update</code>命令的关键字。只是没有<code>nothing</code> 这个包导致的结果。如果你输入<code>foobar</code>，结果也一样。  
如果你用的Composer版本足够新，那么你可以直接使用<code>--lock</code>选项：  
<pre>
composer <span class="string">update --lock</span>
</pre>


###2. 不编辑composer.json的情况下安装库
你可能会觉得每安装一个库都需要修改composer.json太麻烦，那么你可以直接使用require命令。  
<code>
composer require "foo/bar:1.0.0"
</code>

这个方法也可以用来快速地新开一个项目。<code>init</code>命令有<code>--require</code>选项，可以自动编写<code>composer.json</code>：（注意我们使用<code>-n</code>，这样就不用回答问题）  
<pre>
<code>
$ composer init --require=foo/bar:1.0.0 -n
$ cat composer.json
</code>
<code>
{
	"require": {
		"foo/bar": "1.0.0"
	}
}
</code>
</pre>


###3. 派生很容易
初始化的时候，你试过<code>create-project</code>命令么  
<code>
composer create-project doctrine/orm path 2.2.0
</code>
这会自动克隆仓库，并检出指定的版本。克隆库的时候用这个命令很方便，不需要搜寻原始的URI了。  


###4. 考虑缓存，dist包优先
最近一年以来的Composer会自动存档你下载的dist包。默认设置下，dist包用于加了tag的版本，例如<code>"symfony/symfony": "v2.1.4"</code>，或者是通配符或版本区间，<code>"2.1.*"</code>或<code>">=2.2,<2.3-dev"</code>（如果你使用<code>stable</code>作为你的<code>minimum-stability</code>。  

dist包也可以用于诸如<code>dev-master</code>之类的分支，Github允许你下载某个git引用的压缩包。为了强制使用压缩包，而不是克隆源代码，你可以使用<code>install</code>和<code>update</code>的<code>--prefer-dist</code>选项。  

下面是一个例子（我使用了<code>--profile</code>选项来显示执行时间）：  
<pre>
$ composer init --require="twig/twig:1.*" -n --profile
Memory usage: 3.94MB (peak: 4.08MB), time: 0s

$ composer install --profile
Loading composer repositories with package information
Installing dependencies
  - Installing twig/twig (v1.12.2)
    Downloading: 100%

Writing lock file
Generating autoload files
Memory usage: 10.13MB (peak: 12.65MB), time: 4.71s

$ rm -rf vendor

$ composer install --profile
Loading composer repositories with package information
Installing dependencies from lock file
  - Installing twig/twig (v1.12.2)
    Loading from cache

Generating autoload files
Memory usage: 4.96MB (peak: 5.57MB), time: 0.45s
</pre>
这里，<code>twig/twig:1.12.2</code>的压缩包被保存在<code>~/.composer/cache/files/twig/twig/1.12.2.0-v1.12.2.zip</code>。重新安装包时直接使用。


###5. 考虑修改，源代码优先
当你需要修改库的时候，克隆源代码就比下载包方便了。你可以使用<code>--prefer-source</code>来强制选择克隆源代码。  
<pre>
<code>composer</code> update symfony/yaml --prefer-source
</pre>

接下来你可以修改文件：
<pre>
composer status -v
You have changes in the following dependencies:
/path/to/app/vendor/symfony/yaml/Symfony/Component/Yaml:
    M Dumper.php
</pre>

当你试图更新一个修改过的库的时候，Composer会提醒你，询问是否放弃修改：
<pre>
$ composer update
Loading composer repositories with package information
Updating dependencies
  - Updating symfony/symfony v2.2.0 (v2.2.0- => v2.2.0)
    The package has modified files:
	M Dumper.php
	Discard changes [y,n,v,s,?]?
</pre>


#为生产环境作准备
最后提醒一下，在部署代码到生产环境的时候，别忘了优化一下自动加载：
<pre>
<code>composer</code> dump-autoload --optimize
</pre>

安装包的时候可以同样使用<code>--optimize-autoloader</code>。不加这一选项，你可能会发现[20%到25%的性能损失](http://www.ricardclau.com/2013/03/apc-vs-zend-optimizer-benchmarks-with-symfony2/)。  

如果你需要帮助，或者想要了解某个命令的细节，你可以阅读[官方文档](http://getcomposer.org/)，或者查看JoliCode做的这个[交互式备忘单](http://www.ricardclau.com/2013/03/apc-vs-zend-optimizer-benchmarks-with-symfony2/)。

#八.Composer更换国内镜像
- 安装composer，不再赘述

- 修改配置，使用中国镜像 先找到config文件
`sudo composer config -l -g`
  
![composer-modify-image](/assets/postImage/php/composer-modify-image-1.png "composer-modify-image")
--
修改配置文件
`$ sudo vim /root/.composer/config.json`
增加镜像地址
``{
	"repositories": [
		{"type": "composer", "url": "http://pkg.phpcomposer.com/repo/packagist/"},
		{"packagist": false}
	]
}``  
修改完成，保存退出  
![composer-modify-image](/assets/postImage/php/composer-modify-image-2.png "composer-modify-image")
--

- 更新composer
<code>
$ composer self-update
</code>

- 创建自己的composer项目
<pre>
# 创建项目目录
<code>$ mkdir project</code>
<code>$ cd project</code>
# 增加依赖引入swoole_framework框架包，当然也可以是其他包
<code>$ composer require "matyhtf/swoole_framework:dev-master"</code>
</pre>




[php composer](http://www.phpcomposer.com/)  
[getcomposer](https://getcomposer.org/)  
[php composer docs](http://docs.phpcomposer.com/)

