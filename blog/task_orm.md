<!--
author: jibo
date: 2016-05-28
title: 任务 - 建立数据库表的映射对象
tags: fanhua
category: Task
status: publish
summary:
-->

###任务描述###
请参考我给出的例子，为数据库表建立映射对象，同时包括基本的单元测试
1. 每张表对应一个 data 映射对象，一个 repository 对象，以及一个单元测试文件
2. 对象字段的命名，数据库字段为汉语拼音的，直接使用汉语拼音，如字段 xuhao，在 java 类中映射为 String xuhao；字段为英文单词的，使用小写驼峰命名法，如 firstclassify 映射为 String firstClassify
3. oracle 数据类型 VARCHAR2, NVARCHAR2 等映射为 String，Interger 映射为 Long，Float 映射为 Double 等，在 java 映射文件中不使用基本数据类型
4. 总共37张表，示例针对表 fh_biaozhunjian；请参考示例，首先对表 fh_caigouyongkuanshenqing 建立映射对象，完成后发给我检查一下；经验证后再处理剩余的35张表

---
###示例说明###
戳这里：[src][1]
  [1]: https://github.com/syncxplus/spring-boot-demo/archive/oracle.zip

SQL文件：
> src/main/resources/db/migration/V1_0__init.sql

数据库表名：fh_biaozhunjian
java对象：
>路径：src/main/java/com/syncxplus/data
- Biaozhunjian
- BiaozhunjianRepository

>路径：src/test/java/com/syncxplus/data
- BiaozhunjianTest

---
###注意事项###
- 在映射对象文件中，注解 @Column 对应数据库表的列名，必须一模一样
- 每一个数据映射对象至少写一个 testcase
- **请不要**直接编辑 sql 文件，**也不要**登录到oracle中直接修改sql表结构，否则造成 hash 校验出错，会导致运行失败

---

