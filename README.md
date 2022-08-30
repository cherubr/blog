##  blog 文档说明

### 1. 项目目录
  ```
    +-- _archetypes  新文章默认模板    
    +-- _content   存放所有Markdown格式的文章  
    |  +-- post 文章草稿  
    +-- _data  
    +-- _layouts 存放自定义的view，可为空   
    +-- _public  发布后的文件
    +-- _resources  
    +-- _static  存放图像、CNAME、css、js等资源，发布后该目录下所有资源将处于网页根目录  
    +-- _themes 主题  
    |&emsp; +-- maupassant主题
  ```
### 2.创建个人博客（hugo）
- 安装hugo
- hugo new site blog 生成站点信息
- git submodule add ${url} 主题仓库或者下载对应主题放到themes下面。
- 配置对应config.tom文件主题名称
```
theme : ""

```
- hugo 生成public下网站资源信息
- hugo server
