# 0. blog 文档
- 项目目录

# 1. 项目目录
  .  
  +-- _archetypes  新文章默认模板    
  +-- _content   存放所有Markdown格式的文章  
  |&emsp;   +-- post 文章草稿  
  +-- _data  
  +-- _layouts 存放自定义的view，可为空   
  +-- _public  发布后的文件
  +-- _resources  
  +-- _static  存放图像、CNAME、css、js等资源，发布后该目录下所有资源将处于网页根目录  
  +-- _themes 主题  
  |&emsp; +-- maupassant主题  
  
 #2. git submodule 使用
 - 用来初始化本地配置文件
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




