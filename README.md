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
 ###2. git submodule 使用  

 - 添加子仓库  

     git submodule add <仓库地址> <本地路径>  
     git submodule add https://gitee.com/xiaomumaozi/SubModule.git src/SubModule  

 - 如果要clone一个附带submodule的项目，submodule的文件,用来初始化本地配置文件

    git submodule init  

 - 从该项目中抓取所有数据并检出父项目中列出的合适的提交(指定的提交)。  

    git submodule update  

 ------------------更好的方式---------------------

- clone 父仓库的时候加上 --recursive，会自动初始化并更新仓库中的每一个子模块  

- 逆初始化模块，其中{MOD_NAME}为模块目录，执行后可发现模块目录被清空  

 git submodule deinit {MOD_NAME}   

- 删除.gitmodules中记录的模块信息（--cached选项清除.git/modules中的缓存）  

 git rm --cached {MOD_NAME}   

- 提交更改到代码库，可观察到'.gitmodules'内容发生变更  

 git commit -am "Remove a submodule."  
- 执行hugo 命令生成对应public静态网页数据。
- 提交代码先 cd 到public/或者themes/maupassant/目录下先提交module文件，再到blog目录提交对应文件

## markdown语法
- 超链接语法
[超链接显示名](超链接地址 "超链接title")  
不同的 Markdown 应用程序处理URL中间的空格方式不一样。为了兼容性，请尽量使用%20代替空格  
- 网址和Email地址
<https://markdown.com.cn>


## hugo server 使用
- 在config.toml所在目录下执行 hugo server
- hugo 生成public 目录
- hugo server　预览
- hugo -D server 可以预览　draft=false md文件
- 新建文件
- hugo new [path]
- 在content目录下创建一篇新内容文件. path为完整的路径, 包含文件名和扩展名. 以content目录为根目录.  
hugo new site [path]
- 指定的目录下创建网站骨架, 静态网站是根据这些网站骨架生成的.  
hugo new theme [name]

## 创建bolg
- 在本地文件夹使用hugo new site {{blogName}} 生成blog 文件
- git init 初始化本地仓库
- 使用git submodule add 添加你在github 中创建的一个以(名称.github.io)的仓库 删除本地public目录（放置下面命令报错）  
如： git submodule add {url}   public/
- 添加主题 git SubModule add {主题仓库}  
如： git submodule add {url} /themes/{随意名称} （设置config.tom主题和该主题名称保持一致）  
- 提交问题  
- 主目录下 每次使用hugo 生成public 目录下文件。
- 切换到对应public目录进行git提交  
- 同理 主题也是同样设置，  
- 最后提交总的文件
