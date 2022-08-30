---
title: "Frc"  
date: 2020-01-06T22:09:08+08:00  
categories: ["远程桌面"]  
slug: "hugo"  
tags: ["frp", "阿里云配置frp","frs"]  
# draft: true  
---
# 基于frp的远程桌面使用
- 下载frp
- 部署fps
- 部署frc

## 什么是frp？
frp是一种快速反向代理，可帮助您将NAT或防火墙后面的本地服务器公开到Internet。到目前为止，

它支持TCP和UDP以及HTTP和HTTPS协议，在这些协议中，请求可以通过域名转发到内部服务。frp还具有P2P连接模式。

下载地址[frp](https://github.com/fatedier/frp/releases)

## 部署服务端的frp-server
- 这里你需要一个公有的ip（如阿里云服务器）
- 解压之后你会看到如图

![frps123](../assert/frps.png)
- 配置frps.ini文件夹 我的配置如下，
```shell script
[common]
bind_addr = 0.0.0.0
bind_port = 7000

dashboard_addr = 0.0.0.0
dashboard_port = 7500

# dashboard user and passwd for basic auth protect, if not set, both default value is admin
dashboard_user = test
dashboard_pwd = testpasswd

# dashboard assets directory(only for debug mode)
# assets_dir = ./static
# console or real logFile path like ./frps.log
log_file = ./frps.log

# trace, debug, info, warn, error
log_level = info

# auth token
token = passwd
```
- 启动frps-server  
dashboard这个对应的基本上都是网页访问frp Web网页配置
```shell script
nohup ./frps -c ./frps.ini > 2019-01-06.log 2>&1 &
```
- 注意阿里云服务器对应的网络规则需要打开。如果服务器开启了防火墙则需要打开防火墙
```shell script
firewall-cmd status  查看防火墙状态
systemctl start firewall 开启
systemctl stop firewalld 关闭
firewall-cmd --permanent --zone=public --add-port=8888/tcp 添加相对应端口号
firewall-cmd --reload 重新加载
```
- 访问ip+7500使用test，testpasswd登录访问，成功即部署成功
## frc-client部署
- 解压对应的系统的文件找到对应frpc.ini 配置文件
```shell script
[common]
#配置的服务器ip地址（阿里云公网ip）
server_addr = x.x.x.x
#上面配置远程代理的端口号
server_port = 7000 
#配置的密码
token = passwd

[home-rdp]
#传输协议
type = tcp
#本地监听
local_ip = 0.0.0.0
#本地端口号
local_port = 3389
#远程连接访问的端口号
remote_port = 7300
```
- 直接启动cmd下面运行
```shell script
./frpc -c ./frpc.ini
```
- 配置window-server启动
- 下载winsw
下载地址[winsw](https://github.com/kohsuke/winsw/releases)
下载编译之后的文件 如图  
![winsw](../assert/winsw.png)  
解压之后将其放到frc当前的文件夹下面重命名为winsw.exe
- 添加一个winsw.xml文件
```xml
<!--
    Copyright (c) 2016 Oleg Nenashev and other contributors

    Permission is hereby granted, free of charge, to any person obtaining a copy of this 
    software and associated documentation files (the "Software"), to deal in the Software without
    restriction, including without limitation the rights to use, copy, modify, merge, publish,
    distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the
    Software is furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in all copies or 
    substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING 
    BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
    NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, 
    DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, 
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
-->

<!--
 This is an example of a minimal Windows Service Wrapper configuration, which includes only mandatiory options.
 
 This configuration file should be placed near the WinSW executable, the name should be the same.
 E.g. for myapp.exe the configuration file name should be myapp.xml
 
 You can find more information about the configuration options here: https://github.com/kohsuke/winsw/blob/master/doc/xmlConfigFile.md
 Full example: https://github.com/kohsuke/winsw/blob/sample-config-file/examples/allOptions.xml
-->
<configuration>
  
  <!-- ID of the service. It should be unique accross the Windows system-->
  <id>frpc</id>
  <!-- Display name of the service -->
  <name>Frpc Service (powered by WinSW)</name>
  <!-- Service description -->
  <description>This frpc service can realize reverse Proxy</description>
  <!-- Path to the executable, which should be started -->
  <executable>frpc</executable>
  <arguments>-c frpc.ini</arguments>
  <logmode>reset</logmode>

</configuration>
```
- 启动命令
```shell script
winsw install
winsw start
#或者
.\winsw install
.\winsw start
```
- 远程访问计算机 

名称：变成了公有服务器ip端口号

用户名：计算机名称

密码： 计算机登录密码






