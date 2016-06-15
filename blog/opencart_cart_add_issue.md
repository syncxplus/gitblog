<!--
author: jibo
date: 2016-06-15
title: Opencart 添加到购物车说明
tags: php, opencart
category: Notes
status: publish
summary:
-->

##@ZhouYi

我只有原始的代码，所以说明一下原始代码的逻辑：

添加购物车（$('#button-cart').click）时，页面发送的json参数是通过jquery选择器，选择了一系列input控件，将每个控件的name和value作为一个参数，以尺码为例：
![此处输入图片的描述][1]
可以看到，尺码实际对应select控件（name="option[922]"），选中的尺码US5=EU35实际对应value为3005，因此该参数为{option[922]:3005}

查看页面发送的参数，可以验证上述判断
![此处输入图片的描述][2]

再看opencart的源代码，可以验证上述判断，#product select这一项，对应了上述参数
![此处输入图片的描述][3]
这里的jqeury选择器中包含了一系列表达式，用逗号分隔，每一个表达式选中的对象，都会按照name:value添加到json参数之中

**现在看不到你修改之后的逻辑，接下来属于我的猜测和建议**

如果你没有修改提交参数的方式，依然使用上述jquey选择器；但是由于修改页面样式，尺码不再是select控件；那么你可以添加一个hidden的input，当选中尺码样式的时候，动态设置该input的value，类似

    <input id="size" type="hidden" name="option[922]" value="3005"/>
然后在jquery的选择器中将 #product select 替换为 #size

如果你完全修改了逻辑，使用了新的方式提交参数，那么只要你提交的参数name为option["922"]，value为选定的尺码对应的数值即可

注意 option["922"] 这个参数名也是动态的，在页面中可以看到
![此处输入图片的描述][4]
*即是说，需要将option[<?php echo $option['product_option_id']; ?>]作为参数名，选中的尺码<?php echo $option_value['product_option_value_id']; ?>作为参数值，发送到服务器*


  [1]: img/opencart/4.png
  [2]: img/opencart/3.png
  [3]: img/opencart/1.png
  [4]: img/opencart/2.png

