---
layout: post
title:  "jekyll 使用教程"
category: jekyll
comments: true
date:   2014-09-05 09:18:51
---

### 使用jekyll建立博客

ps：如果你还没有安装Jekyll，请转到 [Jekyll安装教程](/jekyll/2014/09/05/ubuntu-install-jekyll.html)
========

#### 1.jekyll 初始化Blog  
<pre><code>jekyll new myblog	#初始化blog系统
jekyll server	#启动jekyll内置的服务,jekyll会监听本地的4000端口号;执行此命令jekyll会自动在 _site/目录下生成blog静态html文件</code></pre>

#### 2.jekyll 目录结构  
ps：[官网解释](http://jekyllcn.com/docs/structure/)  
<pre>
|-- _includes		#存放一些公共的主题模板,和一些常用的工具,评论等
|-- _layouts		#布局,一般会include/下的模板片段
|   |-- default.html
|   `-- post.html
|-- _plugins		#插件目录,即：jekyll的插件存放在此目录
|-- _posts		#存放数据,我用的是Markdown格式文件用来存储数据
|   |-- 2007-10-29-why-every-programmer-should-play-nethack.textile
|   `-- 2009-04-26-barcamp-boston-4-roundup.textile
|-- _sass		#存放css编译之始文件, 一般是 *.scss格式的文件
|-- css		#_sass文件夹编译之后的css文件
|-- _site		#html文件生成在此目录
|-- assets		#自己定义的文件夹,用来存放image,css,js等文件
|   |--  css
|   |--  image
|   `--  js
`-- index.html		#blog_入口文件
|-- _config.yml		#配置文件
`-- feed.xml		#xml
`-- about.md		#about文件
`-- README.md		#README文件
</pre>

#### 3.jekyll 配置  
1.我的jekyll配置：  
ps：不再过多解释,[官网配置](http://jekyllcn.com/docs/configuration/)
<pre>
title: 邹帅的技术博客
email: zoushuai518@gmail.com
description: this zoushuai technology blog
baseurl: "" # the subpath of your site, e.g. /blog/
url: "http://zoushuai518.github.io" # the base hostname & protocol for your site
twitter_username: jekyllrb
github_username:  jekyll
safe: false
pygments: true
markdown: kramdown
paginate: 5
</pre>

以下配置为网络收集,仅供参考：  
<pre>
permalink: /:year/:month/:day/:title.html   #博文的固定链接
paginate: 10                                #分页时每页博文数量
author:                                     #自定义常亮
name: zoushuai
email: zoushuai518@gmail.com
link: http://zoushuai518.github.io
title: zoushuai的博客                             #自定义常量
locals:                                     #自定义常量
tags: 标签
about: 关于
active: 技术                                 #自定义常量
subscribe_rss: /pages/atom.xml              #订阅地址
markdown: redcarpet                         #markdown解释器
logo：网站logo
search：是否运行搜索
encoding：编码
timezone：时区
</pre>

#### 4.jekyll 常用命令  
快速开始：
<pre>
 ~ $gem install jekyll
 ~ $jekyll new my-awesome-site
 ~ $cd my-awesome-site
 ~ /my-awesome-site $ jekyll serve
 ~ Now browse to http://localhost:4000
</pre>

基本用法：  

1.Jekyll 同时也集成了一个开发用的服务器，可以让你使用浏览器在本地进行预览：  
ps：[Usage](http://jekyllcn.com/docs/usage/)  
<pre>
$ jekyll serve
# => 一个开发服务器将会运行在 http://localhost:4000/

$ jekyll serve --detach
# => 功能和`jekyll serve`命令相同，但是会脱离终端在后台运行。
#    如果你想关闭服务器，可以使用`kill -9 1234`命令，"1234" 是进程号（PID）。
#    如果你找不到进程号，那么就用`ps aux | grep jekyll`命令来查看，然后关闭服务器。[更多](http://unixhelp.ed.ac.uk/shell/jobz5.html).

$ jekyll serve --watch
# => 和`jekyll serve`相同，但是会查看变更并且自动再生成。
</pre>

2.安装了 Jekyll 的 Gem 包之后，就可以在命令行中使用 Jekyll 命令了。有以下这些用法：  
<pre>
$ jekyll build
# => 当前文件夹中的内容将会生成到 ./_site 文件夹中。

$ jekyll build --destination <destination>
# => 当前文件夹中的内容将会生成到目标文件夹<destination>中。

$ jekyll build --source <source> --destination <destination>
# => 指定源文件夹<source>中的内容将会生成到目标文件夹<destination>中。

$ jekyll build --watch
# => 当前文件夹中的内容将会生成到 ./_site 文件夹中，
#    查看改变，并且自动再生成。
</pre>


