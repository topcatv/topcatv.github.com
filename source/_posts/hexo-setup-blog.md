title: 使用Hexo来创建个人博客
date: 2013-09-08 20:45:23
categories: [hexo]
tags: [hexo, github, 博客]
---

# 安装Hexo

1.安装Git
``` bash
$ sudo apt-get install git-core
```
<!-- more -->

2.安装node.js

- 首先安装nvm
``` bash
$ curl https://raw.github.com/creationix/nvm/master/install.sh | sh
```

- 通过nvm安装node.js **(注意版本一定要是0.10以上)**
``` bash
$ nvm install 0.10
```

3.安装使用hexo

- 安装hexo
``` bash
$ npm install -g hexo
```

- 初始化
``` bash
$ hexo init <folder>
```

# 配置Hexo
1.申请账号并获取jiathis和友言的嵌入代码
去[加网](http://www.jiathis.com/)申请一个账号
[友言](http://www.uyan.cc/)
这个账号可以供友言和jiathis使用

2.添加友言
在`_config.yml`文件中添加
```
#友言
uyan:
  enable: true
```

在`themes/light/layout/_partial/comment.ejs`中将其修改为如下所示
``` html
<% if (config.disqus_shortname && page.comments){ %>
<section id="comment">
  <h1 class="title"><%= __('comment') %></h1>
  <div id="disqus_thread">
    <noscript>Please enable JavaScript to view the <a href="//disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
  </div>
</section>
<% } %>

<% if (config.uyan.enable && page.comments){ %>

<section id="comment">
  <h1 class="title"><%= __('comment') %></h1>
  <div id="disqus_thread">
    <!-- UY BEGIN -->
    <div id="uyan_frame"></div>
    <script type="text/javascript" src="http://v2.uyan.cc/code/uyan.js?uid=你的id"></script>
    <!-- UY END -->
  </div>
</section>

<% } %>
```
3.添加分享
在`themes/light/_config.yml`文件中添加
```
jiathis:
  enable: true
```
在`themes/light/layout/_partial/post/share.ejs`中将其修改为如下所示
```
<% if (theme.addthis.enable){ %>
  <div class="addthis addthis_toolbox addthis_default_style">
    <% if (theme.addthis.facebook){ %>
      <a class="addthis_button_facebook_like" fb:like:layout="button_count"></a>
    <% } %>
    <% if (theme.addthis.twitter){ %>
      <a class="addthis_button_tweet"></a>
    <% } %>
    <% if (theme.addthis.google){ %>
      <a class="addthis_button_google_plusone" g:plusone:size="medium"></a>
    <% } %>
    <% if (theme.addthis.pinterest){ %>
      <a class="addthis_button_pinterest_pinit" pi:pinit:layout="horizontal"></a>
    <% } %>
    <a class="addthis_counter addthis_pill_style"></a>
  </div>
  <script type="text/javascript" src="//s7.addthis.com/js/300/addthis_widget.js<% if (theme.addthis.pubid){ %>#pubid=<%= theme.addthis.pubid %><% } %>"></script>
<% } %>

<% if (theme.jiathis.enable){ %>
<!-- JiaThis Button BEGIN -->
你在jiathis获得的代码
<!-- UJian Button END -->
<% } %>
```
4.添加新浪微博展示widget
到新浪微博的[微博秀](http://app.weibo.com/tool?topnav=1&wvr=5)里获得微博秀的代码
在`themes/light/layout/_widget`下创建weibo.ejs文件并添加
```
<div class="widget tag">
你在新浪微博的微博秀中生成的代码
</div>
```

# 编写博客
新键一篇博客
```
$ hexo new post "your article title"
```
可以到source/_posts/下去找到刚新建的博客进行编辑

# 发布到github
-执行`hexo generate`生成html
-执行`hexo deploy`发布到github
  发布之前先去到`_config.yml`文件中去设定你的github地址
  ```
  deploy:
    type: github
    repository: https://github.com/你的github账号名/你的github账号名.github.com.git
  ```