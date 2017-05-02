title: Hexo博客建站流程备忘
date: 2017-3-20 21:15:36
categories: 博客建设
tags: 
 - hexo
toc: true
comments: true
---
Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。
基于Hexo搭建的博客可以部署在自己的服务器上（腾讯云、阿里云等），也可以部署在github上，部署在github上更为方便，节省成本，管理也方便。
## 环境搭建
事先需要安装：
 - [Node.js](http://nodejs.org/)
 - [Git](http://git-scm.com/)

安装Hexo：

`$ npm install -g hexo-cli`

具体的安装方法可参考[官方文档](https://hexo.io/zh-cn/docs/index.html)。

## 域名设置
我在腾讯云上申请的域名是`xiaojn.cn`，看到[这篇文章](http://blog.fens.me/hexo-blog-github/)中说可以把blog的域名设置为子域名。下面是我的设置内容。
![子域名配置][2]
![CNAME文件][1]
在dnspod控制台，我们要做3步设置。
- 设置主机记录github，类型A，到IP 199.27.76.133
- 设置主机记录evanqiao.github.io，类型CNAME，到github.xiaojn.cn.
- 设置主机记录blog，类型CNAME，到 evanqiao.github.io
记得我们还要修改文件CNAME，改为blog.xiaojn.cn。通过浏览器，访问[http://blog.xiaojn.cn](http://blog.xiaojn.cn)，就可以打开了我们的博客站点了，而这次用的是二级域名。

由于每次执行deploy的时候，gh-pages分支所有的文件都会被覆盖，所以我们最好在source目录下创建这个CNAME文件，这样每次部署就不用动手创建了。
## 主题安装
选了一圈主题，最好还是觉得`maupassant`这个主题比较简洁。最终选择的是`tufu9441`在原来的主题上进行二次开发过的`maupassant`主题。
安装主题和渲染器：
```
$ git clone https://github.com/tufu9441/maupassant-hexo.git themes/maupassant
$ npm install hexo-renderer-jade --save
$ npm install hexo-renderer-sass --save
```
编辑Hexo目录下的 _config.yml，将theme的值改为maupassant。
作者提供的中文使用[教程](https://www.haomwei.com/technology/maupassant-hexo.html)。
需要注意的是：

1. 在配置hexo的_config.yml时，其实是有两个的，一个是hexo的基础配置，主要配置网站相关的信息，一个是对主题的配置，放置在主题对应的目录下面。
2. 在_config.yml中配置Deployment中，type应该是git而不是github,这是由于版本更新导致的。
3. 在安装`hexo-renderer-sass`时可能导致出错，是因为被墙的原因，可以换用国内的[淘宝NPM镜像](http://npm.taobao.org/)。如果没有安装成功的话，首先你需要卸载才可以继续用cnpm继续安装的:`npm uninstall  hexo-renderer-sass --save`不然还会出错,如果你忽略这个错误，那么等你部署上去的时候，你会发现你的网站没有样式。

## 发布到github
在github上创建一个repository，然后在hexo的配置文件_config.yml
中添加部署内容：
```
deploy:
type: git
repo: git@github.com:Evanqiao/xiaojn-blog.git
```
这个静态的web网站就被部署到了github，分支是gh-pages，gh-pages是github为了web项目特别设置的分支。
为了备份博客内容和搭建的hexo环境，我这里把hexo的环境和source/目录下的内容备份到了`git@github.com:Evanqiao/xiaojn-blog.git`的master分支中。
其他关于hexo博客的插件安装、部署、maupassant主题的使用等内容详见参考链接。
  [1]: /images/CNAME.png
  [2]: /images/subdomain_config.png

Ref：
- [Hexo官方文档](https://hexo.io/zh-cn/docs/)
- [maupassant主题的使用：大道至简——Hexo简洁主题推荐](https://www.haomwei.com/technology/maupassant-hexo.html)
- [使用hexo时遇到的小坑](http://rockcoding.com/2016/03/02/hexo/)
- [Hexo在github上构建免费的Web应用](http://blog.fens.me/hexo-blog-github/)