---
layout: post
title: "Jekyll — GitHub Pages 搭建blog"
description: ""
category: Blog搭建
tags: [Jekyll]
---


安装Jekyll
------------

安装Jekyll,可以本地启动serve来预览页面

安装Jekyll需要以下步骤:

####1.安装ruby环境
输入命令:
    `ruby --version`

    ➜  vivianyu.github.com git:(master) ✗ ruby --version
    ruby 2.0.0p481 (2014-05-08 revision 45883) [universal.x86_64-darwin14]

Mac已经自带Ruby,需确认下版本,确保版本在1.9.3或者2.0.0


####2.安装Bundler管理器

Bundler是一个包管理器，如果你想要在本地环境中预览GitHub Pages页面，Bundler可以使得基于Ruby环境的Jekyll管理起来更加简单。安装了这个管理器之后，我们就可以使用bundler命令来安装和管理Jekyll。

安装Bundler命令:

    ➜  vivianyu.github.com git:(master) ✗ gem install bundler
    ERROR:  Could not find a valid gem 'bundler' (>= 0), here is why:
              Unable to download data from https://rubygems.org/ - Errno::ECONNRESET: Connection reset by peer - SSL_connect (https://rubygems.org/latest_specs.4.8.gz)

报错是因为是因为国内网络导致rubygems.org存放在Amazon S3上面的资源文件间歇性连接失败，用国内的RubyGems镜像(参见[http://ruby.taobao.org/](http://ruby.taobao.org/))替换官方镜像，方法如下：

    ➜  vivianyu.github.com git:(master) ✗ gem install bundler --no-ri --no-rdoc --source http://rubygems.org
    ^CERROR:  Interrupted
    ➜  vivianyu.github.com git:(master) ✗ gem sources --remove https://rubygems.org/
    https://rubygems.org/ removed from sources
    ➜  vivianyu.github.com git:(master) ✗ gem sources -a https://ruby.taobao.org/
    https://ruby.taobao.org/ added to sources
    ➜  vivianyu.github.com git:(master) ✗ gem sources -l
    *** CURRENT SOURCES ***

    https://ruby.taobao.org/
    ➜  vivianyu.github.com git:(master) ✗ gem install bundler --no-ri --no-rdoc
    Fetching: bundler-1.11.2.gem (100%)
    ERROR:  While executing gem ... (Gem::FilePermissionError)
        You don't have write permissions for the /Library/Ruby/Gems/2.0.0 directory.
    ➜  vivianyu.github.com git:(master) ✗ sudo gem install bundler --no-ri --no-rdoc
    Password:
    Fetching: bundler-1.11.2.gem (100%)
    Successfully installed bundler-1.11.2
    1 gem installed


####3.安装Jekyll

在当前仓库中新建一个名位 Gemfile 的文件,在其中添加两行:

    source ‘https://ruby.taobao.org’
    gem ‘github-pages’

然后输入命令:

`bundle install`

这样Jekyll就算安装好了，文件夹下也多了一个Gemfile.lock文件。


运行Jekyll
--------------

安装完Bundler后,命令行中输入:

`bundle exec jekyll serve`

就可以在本地将Jekyll跑起来了，这时你可以通过访问 [http://localhost:4000](http://localhost:4000) 来预览你的GitHub Pages。


新建page
--------------

#####新增一篇文章
    rake post title="Hello World"

#####新增一个页面

    rake page name="about.md"

#####创建一个嵌套页面

    rake page name="pages/about.md"

#####创建一个带路径的页面

    rake page name="pages/about"

this will create the file: ./pages/about/index.html

------------------------------------

*小记: 此笔记更新于2016/01/11,于Mac上重新使用Jekyll.*

*vivian.yu*

---

