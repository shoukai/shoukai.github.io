---
layout: post
title:  "Github Pages 搭建笔记"
subtitle: "Jekyll、MWeb、Github.io、七牛云 构建个人博客笔记"
date:   2018-01-08 10:45:13
background: '/img/posts/01.jpg'
---

# Github Pages 构建笔记

## Github Pages
前往[GitHub](https://github.com/)并且创建一个新的repository，命名规则是：username.github.io（username是你的Github用户名，蓝色线部分相同）

![blog-github-create](http://oo6gt25nl.bkt.clouddn.com/blog-github-create.jpg){:height="100%" width="100%"}

clone项目到本地


{% highlight shell %}
git clone https://github.com/username/username.github.io
{% endhighlight %}


制作Hello World 页面

```
cd username.github.io
echo "Hello World" > index.html
```

提交到GitHub，需要配置SSH免密码登录

```
git add --all
git commit -m "Initial commit"
git push -u origin master
```

访问：https://username（GitHub用户名）.github.io ，可以看见个人主页。

## Jekyll

Jekyll是一个简单的免费的Blog生成工具，类似WordPress。但是和WordPress又有很大的不同，原因是jekyll只是一个生成静态网页的工具，不需要数据库支持。但是可以配合第三方服务,例如Disqus。最关键的是jekyll可以免费部署在Github上，而且可以绑定自己的域名。

**安装过程**

1.安装Ruby

Mac有默认的Ruby环境，根据如下命令确认是否正常工作及版本

```
ruby -v
```

2.安装Jekyll

```
sudo gem install jekyll bundler
```
 
3.进入GitHub本地项目，如果不使用模板，可以进行如下命令安装

```
bundle install
```

4.开启Jekyll环境

```
bundle exec jekyll serve
```

5.本地调试
通过http://localhost:4000 ，访问页面。

（实测截图）

## Jekyll 模板

替换Jekyll模板，根据知乎黄玄回答找到[startbootstrap-clean-blog-jekyll](https://github.com/BlackrockDigital/startbootstrap-clean-blog-jekyll)模板。

效果页面可以访问： [Clean Blog](http://blackrockdigital.github.io/startbootstrap-clean-blog-jekyll/)

![blog-github-clean-blog](http://oo6gt25nl.bkt.clouddn.com/blog-github-clean-blog.jpg){:height="100%" width="100%"}

**安装过程**

安装过程可以使用两种方式。

1. 命令方式：详解startbootstrap-clean-blog-jekyll的GitHub页面
2. 覆盖方式：本文采用方式，详细说明如下

下载或者Clone项目startbootstrap-clean-blog-jekyll到本地，覆盖到GitHub.io的本地工程。

修改 _config.yml 文件，下述为需要修改部分，其他保持不变即可。

| 参数 | 说明 | 
| --- | --- | 
| title | 博客名称 |
| email | 通讯方式 |
| description | 博客描述 |
| github_username | GitHub用户名 |
| baserul | 二级路径，可以为"" | 
| url | GitHub.io的URL路径 |

用过命令

```
bundle exec jekyll serve
```
启动本地环境，通过 http://localhost:4000 访问页面。变动提交到GitHub后，可以在 https://username（GitHub用户名）.github.io 看到修改变化。


## 域名

## MWeb

## 图片

## 参考资源

* [GitHub Page](https://pages.github.com/)
* [有哪些简洁明快的 Jekyll 模板](https://www.zhihu.com/question/20223939)
* [Jekyll Page](https://jekyllrb.com/)
* [startbootstrap-clean-blog-jekyll](https://github.com/BlackrockDigital/startbootstrap-clean-blog-jekyll)



