# [Airplake 官方博客](http://airplake.com/)

## 更新日志

* 添加显示作者信息
* 添加评论插件
* 优化样式



## 分支说明

* master 分支 页面展示分支
* source 分支 hexo 源码


## 使用说明

    git clone https://github.com/airplake/airplake.github.io.git

    git checkout source

然后把你写的博客内容放到`source`里面的`_posts`文件夹中。

需要在你的`markdown`开关上加上文章的介绍信息
例如：

    ---
    title: 标题
    date:时间
    categories: 分类  可选(前端开发、后端开发、工具)
    tags:    
      - 标签1
      - 标签2
    ---


如果需要添加本地图片，你需要在`_posts`文件夹中,新建和你文章同名的文件夹，然后将图片放在里面。
例如

      _posts  
        半小时学会webpack
          what-is-webpack.png
        半小时学会webpack.md

使用方法 `![](what-is-webpack.png)`


### 发布

    hexo g
    hexo d

>参考资料
>[hexo](https://hexo.io/)
