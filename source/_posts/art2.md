---
layout: '[layout]'
title: 一个最简单的 express 应用
date: 2017-05-27 09:56:26
comments: true
categories:
- 前端
tags:
- AngularJS
description: 建立一个 lesson1 项目，在其中编写代码。当在浏览器中访问 http://localhost:3000/ 时，输出 Hello World。
---

入门实例，实现一个简单的评论展示以及点赞功能
代码如下：
myController.js代码：

``` bash
<!DOCTYPE html>
<html ng-app="myapp">
<head>
    <meta charset="UTF-8">
    <title>angular入门实例</title>
    <script src="lib/angular.js"></script>
    <script src="controllers/myController.js"></script>
</head>
<body>

    <div ng-controller="myCtrl">
        <h3 ng-model="pageData">{{pageData.title}}</h3>
        <ul>
            <li ng-repeat="item in items">
                <p>{{item.comment}}</p>
                <span>{{item.date | date:"yyyy-MM-dd HH-mm-ss"}}</span>
                <button ng-click="favourEvent($index)">赞 {{item.favour}}</button>
            </li>
        </ul>
    </div>

</body>
</html>


var app = angular.module('myapp', []);
app.controller('myCtrl', function($scope){
    //定义标题
    $scope.pageData = {title: '评论'
    };

    //定义评论数据数组
    $scope.items = [
        {comment: '评论1', date: '1484189597173', favour: 0},
        {comment: '评论2', date: '1484184597173', favour: 0},
        {comment: '评论3', date: '1484129597173', favour: 0},
        {comment: '评论4', date: '1484184597173', favour: 0},
        {comment: '评论5', date: '1484189595173', favour: 0}
    ];

    /**
     * 点击赞
     * @param index 当前数组的索引值
     */
    $scope.favourEvent = function(index){
        $scope.items[index].favour++;
    }
})
```