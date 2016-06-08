<!--
author: jibo
date: 2016-04-13
title: Mac OS X EI Capitan 配置 xdebug, mongo
tags: mac, php
category: Notes
status: publish
summary:
-->

## Prepare

在 EI Capitan 中引入 SIP 加强了权限控制，关闭方法为：

- 重启系统进入 Recover 模式
- 打开 Terminal 界面，输入 csrutil disable

## Steps

### 配置头文件

创建指向系统自带 php 库的软链接：

```shell
sudo ln -s \
  /Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/usr/include/php \
  /usr/include/php
```

### 编译安装 xdebug

- 编译 xdebug.so <https://github.com/xdebug/xdebug>

- 复制文件 /etc/php.ini.default 到 /etc/php.ini，在配置文件末尾添加

```shell
[Xdebug]
zend_extension="/path/to/your/xdebug.so"
xdebug.auto_trace=On
xdebug.collect_params=On
xdebug.collect_return=On
xdebug.trace_output_dir="path/to/your/dir"
xdebug.profiler_enable=On
xdebug.profiler_output_dir="/path/to/your/dir"
xdebug.idekey=PhpStorm
xdebug.remote_enable=on
xdebug.remote_host=localhost
xdebug.remote_port=9000
xdebug.remote_handler=dbgp
```

- 重启 apache 服务器，在 phpinfo() 中如果能看到 xdebug 的模块，说明配置成功

### 编译安装 mongo driver

**注意：**f3 中使用的扩展为 [mongo.so](http://pecl.php.net/package/mongo)，不是 mongodb.so

有可能编译时找不到 pcre.h 和 openssl/evp.h，可以使用 brew install pcre, openssl，并链接到 /usr/local/include

将生成的 mongo.so 添加到配置文件中
