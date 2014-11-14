---
layout: post
title:  "jekyll 安装教程"
category: jekyll
comments: true
date:   2014-09-05 09:18:51
---

#Ubuntu Jekyll安装

##1. 准备工作:  

1>.安装开发编译环境  
`sudo apt-get install build-essential`

2>. 用apt-get 来安装libyaml  
`sudo apt-get install build-essential bison libyaml-dev`

3>. 安装zlib  
`apt-get install zlib1g-dev`

4>. 安装sqlite3  
`sudo apt-get install sqlite3 libsqlite3-dev`

5>. 安装js运行环境  
`sudo apt-get install nodejs`

6>. 安装libssl  
`sudo apt-get install libssl-dev`


##2. Ruby安装过程:  

1>. 安装ruby  
<span class="impor">http://rubyonrails.org/download  
下载源码 ftp://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p0.tar.gz  
ruby v1.9.2 之后打包有rubygems,不用自己动手安装了, ruby -v,gem -v查看是否安装成功</span>
<pre><code>
tar xvzf ruby-1.9.2-p0.tar.bz2
cd ruby-1.9.2-p0
./configure -prefix=/usr/local/ruby  #指定安装路径
make && sudo make install
sudo ln -s /usr/local/ruby/bin/ruby /usr/bin/ruby
sudo ln -s /usr/local/ruby/bin/gem /usr/bin/gem
</code></pre>

2>. 安装openssl  
<pre><code>
cd ruby-1.9.2-p0/ext/openssl
ruby ruby extconf.rb
make
sudo make install
</code></pre>

3>. 安装zlib  
<pre><code>
cd ruby-1.9.2-p0/ext/zlib
ruby extconf.rb
make
sudo make install
</code></pre>

4>. 改变安装源,突破墙限制  
<pre><code>
gem sources --remove http://rubygems.org/
gem sources -a http://ruby.taobao.org/
gem sources -l
</code></pre>

5>. 安装jekyll:  
<pre><code>
sudo gem install Jekyll
jekyll -v	#查看jekyll版本
gem install rdiscount		#安装Rdiscount，这个用来解析Markdown标记的包
jekyll new myblog		#使用jekyll初始化blog_系统
jekyll serve			#到工程目录启动服务。输入：http://localhost:4000
</code></pre>

<span class="impor">注：jekyll的运行，基于ruby & nodejs。所以，必须安装Ruby & nodejs ; 两者缺一不可,否则，即使jekyll安装成功，也无法运行</span>


##3. Ruby rails安装，使用。ext demo

1>. 安装rails  
<pre><code>
gem install rails
gem install sqlite3 -v '1.3.7'
sudo ln -s /usr/local/ruby/bin/rails /usr/bin/rails
sudo ln -s /usr/local/ruby/bin/bundle /usr/bin/bundle
</code></pre>

2>. 创建一个简单的demo应用:  
首先创建一个你自己的工作目录，我的是：/root/ruby/apps，然后进入cd /root/ruby/apps  
命令行输入：rails new demo --skip-bundle,等一下完成后  
进入到demo目录，找到Gemfile文件，打开，将第一行：source 'https://rubygems.org '  
修改为：source 'http://ruby.taobao.org/'  
进入到demo目录，执行命令bundle install

3>. 运行应用
只要你再输入最后一个命令，你就可以通过浏览器运行你的app了--
demo目录下命令行输入：rails server--
看到了吗？你的rails应用程序服务器启动成功了，赶快打开浏览器体验一下吧：--
输入：http://localhost:3000

