---
layout: post
title: "配置 Git 和 SSH 密钥连接 Github"
category: tool
comments: true
date:   2014-10-07 13:41:51
---

###1.准备工作
安装 git  
sudo apt-get install git  
sudo apt-get install gitk  

###2.生成SSHKeys
生成 SSH Keys
以 Linux (Ubuntu) 为例，需要用到git的 ssh-keygen 命令：

ssh-keygen -t rsa -C "zoushuai518@gmail.com" -f ~/.ssh/csser-github

简单介绍下参数含义：
-t 指定密钥类型，默认即 rsa ，可以省略
-C 设置注释文字，比如你的邮箱
-f 指定密钥文件存储文件名，会生成 csser-github 和 csser-github.pub 两个密钥文件
回车后，遇到提示输入 yes 即可，剩下一路回车，密钥文件就在指定路径下生成了。

###3.将SSH公钥添加到Github
将 SSH 公钥添加到 Github(或者其它的git server)
登录 Github 帐号，找到帐号设置 -> SSH Keys
点击 Add New SSH Key
将本地生成的公钥文件（csser-github.pub）中的文字全选复制到 key 栏，点击 add key 保存。

###4.本地添加SSH别名
本地添加 SSH 别名
如果本机有其它密钥，连接 github 时可能不会自动使用刚生成的密钥，需要设置别名：

$ sudo vi ~/.ssh/config
加入类似的一段代码：
host csser-github
user git
hostname github.com
port 22
identityfile ~/.ssh/csser-github
保存退出。

###5.测试连接
$ssh -T csser-github
Hi csser! You've successfully authenticated, but GitHub does not provide shell access.
表示设置的 SSH Keys 认证通过，但 Github 不提供 shell 访问。
此时就可以正常使用 Github 了'

<span class="impor">url是https的时候会采用用户名认证。 是ssh地址的时候才会采用ssh认证,即密钥认证,不再是用户名密码认证方式</span>
