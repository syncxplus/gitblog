<!--
author: jibo
date: 2016-06-02
title: CentOS 6.7 安装 Mysql 5.6.30
tags: centos, mysql
category: work
status: publish
summary:
-->

---
Centos 6.7 中yum install默认安装的mysql版本为5.1.73，当前项目需要将其替换为5.6.30，步骤如下：

下载官方安装包
```
wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-5.6.30-1.el6.x86_64.rpm-bundle.tar
wget http://dev.mysql.com/get/Downloads/MySQL-5.6/MySQL-shared-compat-5.6.30-1.el6.x86_64.rpm
```
在删除默认版本之前，需要安装MySQL-shared-compat，避免直接删除造成系统报错
```
rpm -i MySQL-shared-compat-5.6.30-1.el6.x86_64.rpm
```
此时查看系统mysql已经包含新版本信息
```
rpm -qa|grep -i mysql
```

>MySQL-shared-compat-5.6.30-1.el6.x86_64

>mysql-libs-5.1.73-5.el6_6.x86_64

删除默认的旧版本
```
yum remove mysql-libs
```
将官方Mysql安装包解压，然后安装server和client
```
tar xvf MySQL-5.6.30-1.el6.x86_64.rpm-bundle.tar
rpm -ivh MySQL-server-5.6.30-1.el6.x86_64.rpm
rpm -ivh MySQL-client-5.6.30-1.el6.x86_64.rpm 
```
启动mysql
```
service mysql start
```
运行**/usr/bin/mysql_secure_installation**，根据提示设置新密码和其他选项，其中需要输入的随机密码，在server安装完成时的显示中可以找到

最后，检查mysql是否已加入启动项
```
chkconfig --list mysql
```

