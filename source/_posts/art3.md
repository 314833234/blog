---
layout: '[layout]'
title: 学习使用外部模块
date: 2017-06-01 09:32:36
categories:
- 前端
tags:
- VueJS
description: 当在浏览器中访问 http://localhost:3000/?q=alsotang 时，输出 alsotang 的 md5 值，即 bdd5e57b5c0040f9dc23d430846e68a3。
---
app.js
``` bash
var myApp = angular.module('starter', ['ionic','oc.lazyLoad’]);

myApp.config(function($stateProvider,
                 $urlRouterProvider,
                 $provide,
                 $compileProvider,
                 $controllerProvider,
                 $filterProvider) {
    //加载配置
    myApp.controller = $controllerProvider.register;
    myApp.directive = $compileProvider.directive;
    myApp.filter = $filterProvider.register;
    myApp.factory = $provide.factory;
    myApp.service = $provide.service;
    myApp.constant = $provide.constant;

    //注册路由
    gFunRegisterAppBasePageStateData($stateProvider.state);
    // if none of the above states are matched, use this as the fallback
    $urlRouterProvider.otherwise('/login');
});
```

oclazyload.config.js
``` bash
myApp.constant('Modules_Config', [
  {
    name:"login”,
    module:true,
    files:["view/controllers/account/signInController.js”]
  },
  {
    name:"signIn”,
    module:true,
    files:["view/controllers/account/signUpController.js”]
  }]);

myApp.config(["$ocLazyLoadProvider","Modules_Config",routeFn]);

function routeFn($ocLazyLoadProvider, Modules_Config){
  $ocLazyLoadProvider.config({
    debug:false,
    events:false,
    modules:Modules_Config
  })
}
```

router.config.js
``` bash
var _appAccountRoutesCfg = [
  {
    "pageId": "0001”,
    "pageDesc": "登录”,
    "pageState": "login”,
    "pageUrlObject": {
        "url": "/login”,
        "templateUrl": "view/html/account/signIn.html”,
        "resolve": {
            "deps": ["$ocLazyLoad",function($ocLazyLoad){
                return $ocLazyLoad.load("login").then(
                    function(){
                        return $ocLazyLoad.load("view/controllers/account/signInController.js”);
                    }
                );
            }]
        }
    }
  },
  {
    "pageId": "0002”,
    "pageDesc": "注册”,
    "pageState": "signIn”,
    "pageUrlObject": {
        "url": "/signIn”,
        "templateUrl": "view/html/account/signUp.html”,
        "resolve": {
            "deps": ["$ocLazyLoad",function($ocLazyLoad){
                return $ocLazyLoad.load("signIn").then(
                    function(){
                        return $ocLazyLoad.load("view/controllers/account/signUpController.js”);
                    }
                );
            }]
        }
    }
  }
];
```
myPageRouteManagement.js
``` bash
/**
 * 将配置的数据写入全局的路由数据缓存中
 *
 * @param cfgPageStateDataList
 */
var gVarAppCfgPageStateDataList = [];
/**
 * 获取所有配置路由数据
 * @param cfgStateDataList
 */
function gFunSaveCfgPageStateDataToBuffer(cfgStateDataList) {
    for (var i=0; i < cfgStateDataList.length; i++) {
        gVarAppCfgPageStateDataList.push(cfgStateDataList[i]);
    }
}

function gFunRegisterPageStateDataToStateProvider(appCfgPageStateData, registerPageStateFunctionHandle) {
  registerPageStateFunctionHandle(appCfgPageStateData.pageState, appCfgPageStateData.pageUrlObject);
}

function gFunRegisterAppBasePageStateData(registerPageStateFunctionHandle) {
    for (var index = 0; index < gVarAppCfgPageStateDataList.length; index++) {
    //注册页面路由
    gFunRegisterPageStateDataToStateProvider(gVarAppCfgPageStateDataList[index], registerPageStateFunctionHandle);
    }
}
```
