---
title: "Hk videos"  
date: 2019-12-27T11:14:50+08:00  
categories: ["JS Web Interface"]  
slug: "hugo"  
tags: ["海康web3.0", "视频接口", "JS for循环闭包问题"]  
<!-- draft: true -->
---
# 海康视频web3.0控件的集成 
- 测试demo的使用
- 集成开发
- 注意事项 


## 1.测试demo
- 1.1下载对应的web开发包[hk-web3.0](https://github.com/Wu-Qin-Hao/HIK-web3.0)

- 1.2安装对应的控件，直接本地打开demo.html文件对相应的数据源进行调试。（如图）  
![hkvideo](../assert/hkvideo/hkvideo.png)
- 1.3点击登录会看到登录日志  
![hkvideo2](../assert/hkvideo/hkvideo2.png)
- 1.4然后点击预览，在正确的登录成功后会看到预览的登录视频。  
![hkvideo3](../assert/hkvideo/hkvideo3.png)

## 2.集成开发

#### 2.1引入对应的js文件
```html
<script type="text/javascript" src="./js/jquery-1.7.1.min.js"></script>
<script type="text/javascript" src="./js/webVideoCtrl.js"></script>
```
#### 2.2开发过程，接口的doc文档中包含了所有的流程，详细问题看接口文档

- 2.2.1初始化
```js
    $(function () {
        // 检查插件是否已经安装过
        var iRet = WebVideoCtrl.I_CheckPluginInstall();
        if (-2 == iRet) {
            notify("该浏览器不支持NPAPI插件！", "warn");
            return;
        } else if (-1 == iRet) {
            alert("您还未安装过插件，请在视频监控菜单下下载安装！");
            return;
        }

        // 初始化插件参数及插入插件
        WebVideoCtrl.I_InitPlugin('100%', '100%', {
            bWndFull: true,//是否支持单窗口双击全屏，默认支持 true:支持 false:不支持
            //默认窗口3*3
            iWndowType: rowNum,
            cbSelWnd: function (xmlDoc) {
                rowNum = $(xmlDoc).find("SelectWnd").eq(0).text();
            }
        });
        WebVideoCtrl.I_InsertOBJECTPlugin("divPlugin");

        // 检查插件是否最新
        if (-1 == WebVideoCtrl.I_CheckPluginVersion()) {
            alert("检测到新的插件版本，请在视频监控菜单下下载安装！");
            return;
        }

    })
    // 切换窗口的分割形状 这里设置rowNum 只是一个初始值，随便指定(放到你需要更改窗口位置)
    WebVideoCtrl.I_ChangeWndNum(iType);
```
- 2.2.2 登录代码见下面注意事项。

- 2.2.3 播放过程
```js
function play(szIP,index){
        WebVideoCtrl.I_StartRealPlay(szIP, {
            iWndIndex:index,
            //1--主码流，2--子码流
            iStreamType: 1,
            //默认通道号
            iChannelID: 1,
            bZeroChannel: false
        });
        console.log("加载成功")
    }
```
## 3.注意事项
- 3.1登录视频接口模块代码
 ```js
  // login
     function clickLogin(deviceList) {
 
         for (var i = 0; i < deviceList.length; i++) {
             var index = deviceList[i]
             if ("" == index.szIP || "" == index.password) {
                 continue;
             }
             var szPort = port;
             var szUsername = username;
             (function(){
                 var indexWindow = i;
                 var szPassword = index.password;
                 var szIP = index.szIp;
                 var indexWindow = i;
                 WebVideoCtrl.I_Login(szIP,1,szPort,szUsername,szPassword, {
                     success: function (xmlDoc) {
                         console.log("设备"+szIP+"登录成功窗口"+indexWindow+"，成功编码"+xmlDoc)
                         play(szIP,indexWindow)
                     },
                     error:function(data){
                         console.log("设备"+szIP+"登录失败，错误编码"+status+"message"+xmlDoc)
                     }
                 });
             })();
         }
     }
```
- 3.2 主要注意一下for循环中I_login()方法调用过程中如果使用同步，遇到接口登录超时，或者登录过程太长，导致阻塞。
进而使浏览器崩溃问题,如果使用异步执行，由于for循环导致每次执行过程中接口index始终指向最后一个index
采用js闭包解决。  
&emsp;闭包代码模块
```js
 (function(){
                 var indexWindow = i;
                 var szPassword = index.password;
                 var szIP = index.szIp;
                 var indexWindow = i;
                 WebVideoCtrl.I_Login(szIP,1,szPort,szUsername,szPassword, {
                     success: function (xmlDoc) {
                         console.log("设备"+szIP+"登录成功窗口"+indexWindow+"，成功编码"+xmlDoc)
                         play(szIP,indexWindow)
                     },
                     error:function(data){
                         console.log("设备"+szIP+"登录失败，错误编码"+status+"message"+xmlDoc)
                     }
                 });
             })();
```
- 3.3每次加载不同数据源之前对相应的视频源进行停止播放，登出操作。
```js
 for(var i=0;i< rowNum*rowNum;i++){
            var data = WebVideoCtrl.I_GetWindowStatus(i);
            if(data != null){
                var stopStatus = WebVideoCtrl.I_Stop(i);
                if(stopStatus == 0){
                    var logoutStatus =  WebVideoCtrl.I_Logout(data.szIP)
                    if(logoutStatus == 0){
                        console.log("登出成功机器"+data.szIP)
                    }else{
                        console.log("登出失败机器"+data.szIP)
                    }
                }else {
                   console.log("视频停止播放失败"+data.szIP)
                }
            }
        }
```

