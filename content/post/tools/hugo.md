---
title: "Atom"  
date: 2022-08-28T14:18:23+08:00  
categories: ["Tools 安装配置"]  
slug: "hugo"  
tags: ["hugo环境变量配置", "hugo server", "hugo"]  
<!-- draft: true   -->
---
# Atom 文件编辑器
- 什么是Hugo
- Hugo安装
- Hugo常用命令



## 1.什么是Hugo
- Hugo 是最受欢迎的开源静态站点生成器之一。凭借惊人的速度和灵活性，Hugo 使建设网站的乐趣再现

## 2.Hugo安装  
- 下载地址: <https://github.com/gohugoio/hugo/releases>
- 配置环境变量  
将下载文件解压之后，放到自己喜欢文件夹里面配置对应系统环境变量path即可使用
![houg_path](../assert/hugo-环境配置.png)

## 3.Hugo常见使用命令
### new
```
- hugo new [path]
在content目录下创建一篇新内容文件. path为完整的路径, 包含文件名和扩展名. 以content目录为根目录.  
- hugo new site [path]  
指定的目录下创建网站骨架, 静态网站是根据这些网站骨架生成的.  
- hugo new theme [name]  
在themes目录下生成自定义模板  
```
### server
```
- hugo server
hugo提供了一个简版的web服务器, 用于预览静态网站.
默认地址: http://localhost:1313/
虽然有web服务器的功能, 但不建议用于生成环境提供web服务.
- hugo server -p [端口]
修改默认端口  
```
