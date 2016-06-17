<!--
author: jibo
date: 2016-06-17
title: 图像压缩格式小调查
tags: image
category: Notes
status: publish
summary:
-->

正在进行的一个项目中需要从服务器端实时发送大量的图片流到浏览器端，为此调研了几种不同的图片压缩格式，主要特点如下（最终忍受了jpg 图片的体积）：

##flif
> * 压缩算法本身还没有冻结，有可能发生重大调整
> * 目前官方只支持从png转换成 flif，因此需要先将 jpg 转为 png
> * 当前所有浏览器原生不支持，可通过第三方 js [poly-flif][1] 支持
> * 转换速度较慢

##bpg
> * 当前所有浏览器原生不支持，可通过官方 js 支持浏览器显示bpg图片
> * mac 上的测试结果：对于 minicap 的截图(jpg)，bpgenc 处理会出错，使用其他途径得到 jpg 图片则未遇到

##webp
> * chrome 原生支持，其余浏览器需要 js 支持

------

###服务器端
> * 三种库官方均提供 lib 和 command line 工具
> * lib 对外提供 c/c++ 的 API

###前端
chrome, opera 原始支持 webp，其他图片格式浏览器原生不支持
> * poly-flif 提供的 js 插件，能够将 flif 图片绘制到 canvas 上
> * bpg 官方提供的 js，能够将 big 图片绘制到 canvas 上
> * webp-flif 能够将将 webp 图像转换为二进制图片流

  [1]: https://github.com/UprootLabs/poly-flif

