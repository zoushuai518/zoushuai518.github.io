---
layout: post
title: "PHP的ISAPI和FastCGI比较"
category: php
comments: true
date:   2014-10-15 16:35:51
---

1、CGI（通用网关接口/Common Gateway Interface）一般是可执行程序，例如EXE文件，和WEB服务器各自占据着不同的进程,而且一般一个CGI程序只能处理一个用户请求。这样，当用 户请求数量非常多时，会大量占用系统的资源，如内存、CPU时间等，造成效能低下。

2、ISAPI（Internet Server Application Program Interface）是微软提供的一套面向WEB服务的API接口，它能实现CGI提供的全部功能，并在此基础上进行了扩展，如提供了过滤器应用程序接 口。ISAPI应用大多数以DLL动态库的形式使用，可以在被用户请求后执行，，在处理完一个用户请求后不会马上消失，而是继续驻留在内存中等待处理别的 用户输入。此外,ISAPI的DLL应用程序和WEB服务器处于同一个进程中，效率要显著高于CGI。

3、FastCGI是可伸缩架构的CGI开放扩展，其主要行为是将CGI解释器进程保持在内存中并因此获得较高的性能。传统的CGI解释器的反复加载是 CGI性能低下的主要原因，如果CGI解释器保持在内存中并接受FastCGI进程管理器调度，则可以提供良好的性能、伸缩性等。

4.php的两种运行方式:

1>.以 ISAPI 模式运行 PHP 的，这种方式最大的缺点就是稳定性不好，当 PHP 出错的时候，Apache进程也死掉。

2>.以 FastCGI 模式运行 PHP

a>.以 FastCGI 模式运行 PHP 的优点(php-fpm)：

- 首先就是 PHP 出错的时候不会搞垮 Apache，只是 PHP 自己的进程当掉（但 FastCGI 会立即重新启动一个新 PHP 进程来代替当掉的进程）。

- 其次 FastCGI 模式运行 PHP 比 ISAPI 模式性能更好。

- 最后，就是可以同时运行 PHP5 和 PHP4。

b>.FastCGI 模式的一些缺点：

- 用 FastCGI 模式更适合生产环境的服务器，但对于开发用机器来说就不太合适。因为当使用 Zend Studio 调试程序时，由于 FastCGI 会认为 PHP 进程超时，从而在页面返回 500 错误。


从版本 4.3.0 开始，PHP 提供了一种新类型的 SAPI（Server Application Programming Interface，服务端应用编程端口）支持，名为 CLI，意为 Command Line Interface，即命令行接口。顾名思义，该 SAPI 模块主要用作 PHP 的开发外壳应用。CLI SAPI 和其它 SAPI 模块相比有很多的不同之处。值得一提的是，CLI 和 CGI 是不同的 SAPI，尽管它们之间有很多共同的行为。  

CLI SAPI 最先是随 PHP 4.2.0 版本发布的，但仍旧只是一个实验性的版本，并需要在运行 ./configure 时加上 –enable-cli 参数。从 PHP 4.3.0 版本开始，CLI SAPI 成为了正式模块，–enable-cli 参数会被默认得设置为 on，也可以用参数 –disable-cli 来屏蔽。  

从 PHP 4.3.0开始，CLI/CGI 二进制执行文件的文件名、位置和是否存在会根据 PHP 在系统上的安装而不同。在默认情况下，当运行 make 时，CGI 和 CLI 都会被编译并且分别放置在 PHP 源文件目录的 sapi/cgi/php 和 sapi/cli/php 下。可以注意到两个文件都被命名为了 php。在 make install 的过程中会发生什么取决于配置行。如果在配置的时候选择了一个 SAPI 模块，如 apxs，或者使用了 –disable-cgi 参数，则在 make install 的过程中，CLI 将被拷贝到 {PREFIX}/bin/php，除非 CGI 已经被放置在了那个位置。因此，例如，如果在配置行中有 –with–apxs，则在 make install 的过程中，CLI 将被拷贝到 {PREFIX}/bin/php。如果希望撤销 CGI 执行文件的安装，请在 make install 之后运行 make install-cli。或者，也可以在配置行中加上 –disable-cgi 参数。  

以下为 CLI SAPI 和其它 SAPI 模块相比的显著区别：
- 与 CGI SAPI 不同，其输出没有任何头信息。尽管 CGI SAPI 提供了取消 HTTP 头信息的方法，但在 CLI SAPI 中并不存在类似的方法以开启 HTTP 头信息的输出。CLI 默认以安静模式开始，但为了保证兼容性，-q 和 –no-header 参数为了向后兼容仍然保留，使得可以使用旧的 CGI 脚本。在运行时，不会把工作目录改为脚本的当前目录（可以使用 -C 和 –no-chdir 参数来兼容 CGI 模式）。出错时输出纯文本的错误信息（非 HTML 格式）。  

- CLI SAPI 强制覆盖了 php.ini 中的某些设置，因为这些设置在外壳环境下是没有意义的。

[php的SAPI，CLI SAPI，CGI SAPI](/php/2014/10/16/php的SAPI-CLI_SAPI-CGI_SAPI)

