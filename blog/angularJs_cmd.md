<!--
author: zhangxuefeng
date: 2016-09-16
title: AngularJs-指令
tags: javascript
category:javascript
status: publish
summary: Angular学习笔记，关于指令
-->
## 引入

```
<script src="http://cdn.static.runoob.com/libs/angular.js/1.4.6/angular.min.js"></script>
```

- ng-app 指令定义一个 AngularJS 应用程序。
- ng-model 指令把元素值（比如输入域的值）绑定到应用程序。
- ng-bind 指令把应用程序数据绑定到 HTML 视图。

```
<div ng-app="">
 	<p>名字 : <input type="text" ng-model="name"></p>
 	<h1>Hello {{name}}</h1>
</div>
```

## 指令

- ng-app
>ng-app 指令定义了AngularJs的【根元素】。ng-app在网页加载完毕时会自动引导应用程序。

- ng-init
> ng-init 指令通常为AngularJs应用程序定义了【初始值】。通常情况下会使用一个控制器或者一个模块来代替它。

- ng-model
> ng-model指令绑定【HTML元素】到相应程序数据。

```
<div ng-app="myApp" ng-controller="myCtrl">
    名字: <input ng-model="name">
</div>

<script>
var app = angular.module('myApp', []);
app.controller('myCtrl', function($scope) {
    $scope.name = "John Doe";
});
</script>

//双向绑定
<div ng-app="myApp" ng-controller="myCtrl">
    名字: <input ng-model="name">
    <h1>你输入了: {{name}}</h1>
</div>

//用户输入验证
<form ng-app="" name="myForm">
    Email:
    <input type="email" name="myAddress" ng-model="text">
    <span ng-show="myForm.myAddress.$error.email">不是一个合法的邮箱地址</span>
</form>

//应用状态，ng-model 指令可以为应用数据提供状态值(invalid, dirty, touched, error):

<form ng-app="" name="myForm" ng-init="myText = 'test@runoob.com'">
    Email:
    <input type="email" name="myAddress" ng-model="myText" required></p>
    <h1>状态</h1>
    {{myForm.myAddress.$valid}}
    {{myForm.myAddress.$dirty}}
    {{myForm.myAddress.$touched}}
</form>

//CSS类
/*
ng-model 指令根据表单域的状态添加/移除以下类：
ng-empty
ng-not-empty
ng-touched
ng-untouched
ng-valid
ng-invalid
ng-dirty
ng-pending
ng-pristine
*/
<style>
input.ng-invalid {
    background-color: lightblue;
}
</style>
<body>

```

ng-model 指令也可以：
1. 为应用程序数据提供类型验证（number、email、required）。
2. 为应用程序数据提供状态（invalid、dirty、touched、error）。
3. 为 HTML 元素提供 CSS 类。
4.  绑定 HTML 元素到 HTML 表单。


- ng-repeat 

> ng-repeat 指令对于集合中（数组中）的每个项会 克隆一次 HTML 元素。

- 自定义指令

> 除了 AngularJS 内置的指令外，我们还可以创建自定义指令。
你可以使用 .directive 函数来添加自定义的指令。
要调用自定义指令，HTML 元素上需要添加自定义指令名。

```
<body ng-app="myApp">

<runoob-directive></runoob-directive>

<script>
var app = angular.module("myApp", []);
app.directive("runoobDirective", function() {
    return {
        template : "<h1>自定义指令!</h1>"
    };
});
</script>

</body>
```

可以通过以下方式调用
1. 元素名 restrict:'E'
```
<runoob-directive></runoob-directive>
```

2. 属性 restrict:'A'
```
<div runoob-directive></div>
```

3. 类名 restrict:'C'
```
<div class="runoob-directive"></div>
```
4. 注释 restrict:'M',replace : true,
```
<!-- directive: runoob-directive -->
```