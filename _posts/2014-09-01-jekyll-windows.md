---
layout: post
title: "jekyll 在windows下安装的注意事项"
description: ""
category: 
tags: []
---
{% include JB/setup %}

今天下午鼓捣了半天终于把Jekyll在windows下装好了.下面总结一下安装过程中遇到的一些问题,顺带着把安装过程总结以备不时之需.


## Jekyll的优点
此处就不多介绍了,网上google去.

##windows下安装所需要的软件
1.rubyinstaller(http://rubyinstaller.org/downloads/) 
2.同时把和rubyinstaller失陪的dev-kit也下载下来.
3.git的windows客户端(http://msysgit.github.io/)

##安装过程

1.这里主要讲下安装ruby过程中需要注意的问题.
   rubyinstaller的安装目录最好设置在C:\ruby200 --->根据版本号不同会有所区别.
   dev-kit的安装目录最好在C:\DevKit
   安装完成之后,查看Ruby的版本
   ># ruby --version

2.安装好rubyinstaller和dev-kit之后.
   打开ruby的 comman-line ----->进入ruby cmd窗口.按照如下命令的顺序操作.
   ># gem update --system

   更新完成之后,接着安装Jekyll
   ># gem install jekyll  ----------------------> 此步应该会遇到很多问题.
   问题1: 如果你的Rubyinstaller和Dev-kit的版本不对应,一直会出现这种错误,exit cde = 2 .
   解决方法：版本一致就OK,我感觉网上相当一部分同学出现的问题都是由于版本不一致出现的.
   问题2: ruby的资源问题,可以参考这篇文章(http://github.kimziv.com/2013/07/19/how-to-install-ruby-gems-in-china/)
   问题3: DL is deprecated, please use Fiddle cann't locate gem file. 这是一个警告,不是错误.


##使用过程

##1.打开ruby命令行窗口
	更新ruby 并安装RubyGems：# gem update --system
	切换到DevKit目录：       # ruby dk.rb init
	                        # ruby dk.rb review
	                        # ruby dk.rb install
##2.安装Jekyll
	打开ruby命令行窗口       # gem install Jekyll
	如果没有错误             # jekyll --version  

##3.1.上传自己的文章到github.
	首先,你需要注册一个github帐号,并新建一个repo,命名为blog,本文采用的静态网页的模版是基于bootstrap的.
	从git上下载该模版.
	启动git命令行--#> git clone https://github.com/plusjade/jekyll-bootstrap.git jekyll
	               #> cd jekyll/
	启动jekyll#jekyll> jekyll serve
	在浏览器输入：http://localhost:4000/ 能够看到欢迎页面.


##3.2. 编写新页面
	在git命令行:  #> rake post title= "Test Jekyll"
	Jekyll会根据时间生成一个文件：2014-08-17-Test-Jekyll.md
    查看文件内容  #> vim ./post/2014-08-17-Test-Jekyll.md  根据需要在文件填入一些内容.

    然后启动服务器,可能会出现这种错误：invalid byte sequence in GBK / UTF-8
    解决方法:  找到Jekyll的安装目录\lib\ruby\gems\2.0.0\gems\jekyll-1.3.0\lib\jekyll\convertible.rb

    找到这样的一段代码:self.content = File.read_with_options(File.join(base, name),os什么)
    修改为:            self.content = File.read_with_options(File.join(base, name),:encoding => "utf-8")


    再重启服务器就ok了.

##3.3. 发布到git
	在git上新建一个repo,命名为blog.并且把jekyll添加到blog.
	打开git bash:
	              #> git remote set-url origin git@github.com:luozhao/blog.git
	              #> git add . 注意add后面一个空格再加一个点
	              #> git commit -m 'new_post'
	              #> git push origin master
	
	在blog上新建一个分支,用来发布我的文章
	              #> git branch gh-pages
	              #> git checkout gh-pages
	
	修改_config.yml文件,设置BASE_PATH和production_url
	              #> vi _config.yml
	              production_url : http://luozhao.github.io
				  BASE_PATH : /blog
    
	发布我们的项目：
	              #> git add .
	              #> git commit -m 'deploy'
	              #> git push origin gh-pages

	发布之后,等10分钟应该就可以访问了.如果10分钟访问不了,可以再等一会,我刚才就等了超过10分钟,可能延时比较大.
	http://luozhao.github.io/blog/archive.html

------------------------------------
Tips:第一次用，一些格式和字体还没弄好！抽时间调一下格式。



