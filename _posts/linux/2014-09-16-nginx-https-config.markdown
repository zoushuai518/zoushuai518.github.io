---
layout: post
title: "nginx https配置"
category: Linux
comments: true
tags : [https, http]
date:   2014-09-16 18:16:51
---

#nginx使用ssl模块配置HTTPS支持

##前期准备：

- 安装openssl库

- nginx要开启ssl模块，nginx编译时指定–with-http_ssl_module参数即可.
<pre>
以下是本人的 nginx编译安装参数；使用nginx -V查看：
<code>
--sbin-path=/usr/local/nginx/nginx --conf-path=/usr/local/nginx/nginx.conf --pid-path=/usr/local/nginx/nginx.pid --with-http_ssl_module --with-pcre=/home/weiyan/lnmp/pcre-8.34 --with-zlib=/home/weiyan/lnmp/zlib-1.2.8 --with-openssl=/home/weiyan/lnmp/openssl-1.0.1c
</code></pre>

##生成证书和密钥：

1. 新建一个目录用来存放证书和私钥  
`
mkdir -p /usr/local/nginx/ssl/
`
2. 进入目录  
`
cd /usr/local/nginx/ssl/
`
3. 创建服务器私钥，命令会让你输入一个口令  
`
sudo openssl genrsa -des3 -out server.key 1024
`
4. 创建签名请求的证书（CSR） [会提示输入省份、城市、域名信息等，重要的是，email一定要是你的域名后缀的。这样就有一个 csr 文件了，提交给 ssl 提供商的时候就是这个 csr 文件]  
`
sudo openssl req -new -key server.key -out server.csr
`
5. 在加载SSL支持的Nginx并使用上述私钥时除去必须的口令  
`
sudo cp server.key server.key.org
sudo openssl rsa -in server.key.org -out server.key
`
6. 最后标记证书使用上述私钥和证书(CSR) [这里测试，我们自己签发证书，只是浏览器会提示不受信任的站点]  
`
sudo openssl x509 -req -days 365 -in server.csr -signkey server.key -out server.crt
`

## Nginx配置

- 修改Nginx配置文件，让其包含新标记的证书和私钥  
<pre><code>
server {

	listen 443;
	server_name  pay.b5m.com;

	#开启ssl
	ssl on;
	#加载证书
	ssl_certificate /usr/local/nginx/ssl/server.crt;
	#加载私钥
	ssl_certificate_key /usr/local/nginx/ssl/server.key;

	set $web_root /var/www/html/zsdemosite/https;
	
	location / {
		root   $web_root;
		index  index.php;
		#重定向规则
		#try_files $uri $uri/ /index.php?$args;
	}

	error_page   500 502 503 504  /50x.html;
	location = /50x.html {
		root   html;
	}

	# proxy the PHP scripts to Apache listening on 127.0.0.1:80
	# 反响代理
	#location ~ \.php$ {
	#    proxy_pass   http://127.0.0.1;
	#}

	# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
	# zs-config
	location ~ \.php$ {
		root   $web_root;
		fastcgi_pass   127.0.0.1:9000;
		fastcgi_index  index.php;
		#fastcgi_index  new.php;
		fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
		include        fastcgi_params;
	}

	# deny access to .htaccess files, if Apache's document root
	# concurs with nginx's one
	#
	#location ~ /\.ht {
	#    deny  all;
	#}
}
</code></pre>

- 实现80端口重定向到443,(即：http强制跳转到https)  
<pre><code>
server {
	listen 80;
	server_name  pay.b5m.com;

	#url重写
	rewrite ^(.*) https://$server_name$1 permanent;
	#rewrite ^/(.*) https://pay.b5m.com/$1 permanent; #关键代码
}
</code></pre>


##备注
有一些开发PHP框架会根据 $_SERVER['HTTPS'] 这个 PHP 变量是否为 on 来判断当前的访问请求是否是使用 https。为此我们需要在 Nginx 配置文件中添加一句来设置这个变量。遇到 https 链接重定向后会自动跳到 http 问题的同学可以参考一下。  

在  
<pre><code>
location ~ \.php${
	fastcgi_param HTTPS on; # 多加这一句
}
</code></pre>
