 ## 本站主要目录结构说明  
 
    content 　　 所用网站内容文件，MarkDown格式  

    config.toml　首页标题，副标题等 

    public　　　 成果物    引用 https://github.com/bodani/.git submodel 

    themes　     网站主题　引用 https://github.com/bodani/Simple.git  
    
                 layouts/partials/header.html  首页格式

 ## 本站运行操作说明

    ./rundemo.sh  用于临时效果展示　,在开发过程中使用

    ./deploy.sh   生成网页静态文件成果物到public文件，并将public文件内容推送到github .　   

    ./push.sh     将本站修改结果推送到github
    
    ./make 一键更新提交

 ## 本站编写基本操作
 
```
　　hugo new posts/new.md 新建页面
```

## clone 到本地编辑

    git clone --recurse-submodules=public https://github.com/bodani/blog-hugo.git 

## 下载

   文档
   git clone https://github.com/bodani/blog-hugo.git

   主题
   git submodule add https://github.com/bodani/Simple/ themes/simple 
