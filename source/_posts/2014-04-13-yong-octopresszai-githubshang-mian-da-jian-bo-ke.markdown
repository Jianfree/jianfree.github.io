---
layout: post
title: "用Octopress在github上面搭建博客"
date: 2014-04-13 14:24:12 +0800
comments: true
categories: Octopress
---

最近突发奇想，要来弄个博客，由于在github上面注册了账号，Pages功能可以托管静态网页，故选择了在这上面搭建一个blog。我选择了基于Jekyll的Octopress做为搭建程序，虽说Octopress官网有详细的教程，但是搭建的过程中还是遇到了很多问题。
###Octopress 设置
根据官网的设置说明 <http://octopress.org/docs/setup/> 一步一步的操作,当执行
{% codeblock  %}
rake deploy
{% endcodeblock %}
这步操作的时候，出现错误。最后发现是自己在前面 **create repository**的时候,勾选了 
{% img /images/blog/20140413githup.png %}  
导致发布不成功。
<!-- more -->

###个性化设置

####导航栏添加关于栏
添加一个About me导航栏
####自定义404错误页
添加了寻人界面
####添加评论与分享
分享我用的是**JiaThis**,评论我用的是友言，在本地测试的时候，发现页面根本就不会显示评论框，还以为是我代码更改的有问题，用**firebug**调试发现，代码添加的没问题，在本地不会显示，提交到github就正常了。
####设置Categories
新建一篇文章后，如果我在头部categories的属性里面添加了我的分类，rake generate的时候，死活不成功，程序报错。提示这样的信息
{% codeblock%}

Building site: source -> public
Liquid Exception: undefined method `strftime' for nil:NilClass in atom.xml
/Users/henry/.rvm/gems/ruby-1.9.3-p392/gems/jekyll-0.12.0/lib/jekyll/filters.rb:34:in `date_to_string'
/Users/henry/.rvm/gems/ruby-1.9.3-p392/gems/liquid-2.3.0/lib/liquid/context.rb:58:in `invoke'

{% endcodeblock %} 
如果不添加自己的分类，则不会有错误。最后把jekyll-0.12.0/lib/jekyll/filters.rb 代码33行附近的
{% codeblock lang:ruby filters.rb %}
    def date_to_string(date)
      date.strftime("%d %b %Y")
    end
{% endcodeblock %}
改为
{% codeblock lang:ruby filters.rb  %}
    def date_to_string(date)
      if date==nil
        return 
      end
      date.strftime("%d %b %Y")
    end
{% endcodeblock %}
就能正常的编译成功了。但是具体是什么导致这样的情况，没有仔细的去研究。 
####常用命令
{% codeblock lang:bash %}
rake new_post['title']
rake generate
rake deploy
git add .
git commit -m 'you messsage'
git push origin source 
{%  endcodeblock %}


**搭博客不易，写博客不易，且行且珍惜。**

