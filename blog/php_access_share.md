<!--
author: zhangxuefeng
date: 2016-08-26
title: php访问网络共享资源和本地文件
tags: PHP,windows
category:PHP
status: publish
summary: php访问windows共享文件
-->
> PHP访问局域网上其他计算机共享资源的配置：
有A（192.168.1.1） B（192.168.1.2） 两台机子。
在A上装有appache，要访问B的共享资源，如pic_gather。

测试代码：
```
$filename = "//192.168.1.31/pic_gather/figure/1.png";  
$size = filesize($filename);  
echo $size;  
```

### step1 
必须保证pic_gather已经能被访问（其中包括防火墙设置，共享设置，这里就不具体讲了），可以测试下，在电脑的资源管理器（应该这么叫的吧，附上图）上输入\\192.168.1.2\pic_gather，能打开B机上的文件


### step2 
A、B两天计算机必须在一个工作组下面（名称随意自己设置，但必须是一个工作组哦）（XP是 我的电脑->右键->属性->计算机名称->更改->工作组，更改完得重启计算机的）（win是 计算机->右键->属性->高级系统设置->计算机名称->更改->工作组，更改完得重启计算机的）。(ps:作者表示，我没有在相同的组里面，依然可以)

### step3 
B要开启guest。(ps:作者表示，我没有启用来宾模式，依然可以)

### step4 
B上pic_gather的文件夹在共享设定方面要设定为“允许用户更改我的文件（这个可以再第一步就设置好，不设置的话，只能访问不能修改）

### step5 
在A上， cmd里面输入services.msc后，双击apache服务，在“登录”选项卡里面，把运行账号改为登录windows的超级账号（比如Administrator），重启apache服务。(ps 想作者是一个懒人，没有装appache，只是用了集成软件zend，所以服务里面只有appche2.2-zend，其实原理一样的)。