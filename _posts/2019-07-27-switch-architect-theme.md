---
layout: post
title: 使用Architect 主题 
date: 2019-07-27
categories: 博客创建过程
---

默认的网站使用的是**minima**主题,  
如果感觉不喜欢这个主题,还可以自己修改切换为不同的主题.

使用
```
jekyll new myblog
```
命令创建的网站使默认用的是 **minima**主题.  
这个主题的标题栏太小,所以跑网上找了一堆主题,最后找一个自己比较喜欢的主题换上.

[architect](https://github.com/pages-themes/architect)的代码仓库在这里,  
里面的README.md有介绍怎么使用.
1.  修改 `_config.yml`文件的主题,:

    ```yml
    theme: jekyll-theme-architect
    ```

2. 如果要本地调试,则需要安装architect主题相关的插件.Optionally, 修改`Gemfile`文件,加入主题需要的插件:

    ```ruby
    gem "github-pages", group: :jekyll_plugins
    ```
	然后,在同一目录下执行
	```
	bundle install
	```
	这样,系统就会自动安装上相关的插件.

做完这些操作后,使用如下命令启动本地服务器.
```
bundle exec jekyll server
```
使用浏览器打开"http://localhost:4000"看看效果

---

不过很遗憾,这个主题有一点点问题,根目录下的index.md写了一个静态网页,而不是写列举所有文章的脚本.  

一般的博客首页,应该要按时间顺序列出博主post的所有文章,而如果首页做成了一个静态网页的话,用户要通过哪一个途径来阅读博主发布的文章呢?

最后只好靠自己对这个主题进行改造.

### 改造过程

既然 **minima**主题可以列举出所有文章,那就把**minima**主题的列举文章部分抽出来,加到**architect**主题里去.

说干就干,首先要找到**minima**主题的所有文件:
```
bundle show minima
```
通过这个命令,会显示出**minima**主题在系统中的存储位置:
```
/var/lib/gems/2.5.0/gems/minima-2.5.0
```

到这个目录下,可以看到该主题的所有配置文件.
```
├── assets
│   ├── main.scss
│   └── minima-social-icons.svg
├── _includes
│   ├── disqus_comments.html
│   ├── footer.html
│   ├── google-analytics.html
│   ├── header.html
│   ├── head.html
│   ├── icon-github.html
│   ├── icon-github.svg
│   ├── icon-twitter.html
│   ├── icon-twitter.svg
│   └── social.html
├── _layouts
│   ├── default.html
│   ├── home.html
│   ├── page.html
│   └── post.html
```

其中最主要的是`_inclues`和`_layouts`这二个目录,把这二个目录的文件拷到刚才使用`jekyll new myblog`创建的博客系统目录myblog里.

此时,再次运行
```
bundle exec jekyll server
```
启动服务器,在网页端应该能看到列举出的所有博文.


### 原理
考其原理.
因为在myblog根目录里的index.md文件,只有一行内容,定义了使用哪一个类型来显示这个index网页.
```
---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
```
内容里定义了这个网页,会使用命名为"home.html"的布局来显示内容.

这样,服务器会在myblog/_layouts目录下,找到home.html文件来显示.打开home.html文件,可以看到大致如下的内容:
```
 for post in site.posts
   ...
 endfor
```

在**architect**主题里,就是因为缺少了这一段程序,所以不能列出所有的文件.

---

**既有网站换新颜,共享主题资源众.**  
**新旧各有优缺点,揉合二者竟全功.**


