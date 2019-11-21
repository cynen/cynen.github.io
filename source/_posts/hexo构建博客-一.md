---
title: hexo构建博客(一)
date: 2019-11-21 21:32:37
categories:
  - 兴趣爱好
  - 个人博客
tags:
  - hexo
---


最近一直在做项目，好多东西都是新学习到的。遇到问题基本都是百度+google，看着别人的博客，总感觉有些知识点也是自己曾经写过，于是就总想着搭建自己的博客，奈何一直各种内心借口，一直拖到最近，发现 了  https://foxgrin.github.io 的个人博客，，，如是，仔细看了一下，发现然来搭建github博客如此简单。内心痒痒。。。

# 前言

我平时记录笔记基本只用有道笔记的，有道笔记的文件夹分类可以无限分，但是每次想找到自己以前记录的笔记时，总是花比较长的时间去找。其次，有道和word其实比较像的，当然在图片这一块做的不错。然后就想搭建自己的博客，从GitHub开始入手吧，等以后经验老道了，再去搭建自己的博客系统。
刚好，本次也研究了一下，就以搭建GitHub博客作为自己的开篇博文吧~~~

[引用foxgrin](https://foxgrin.github.io/posts/29757/#hexo%E7%8E%AF%E5%A2%83%E6%90%AD%E5%BB%BA)

# 准备事项

1、进入自己的GitHub账号，创建一个 yourname.github.io的Repository 
`项目名称一定要注意使用自己名称.github.io`
`注册GitHub就没啥好讲的,内事不决问百度,外事不决问谷歌`
2、开启github pages
创建对应的Repository

打开对应的仓库的setting项

开启GitHub pages

选择一款自己喜欢的主题（PS：这个可以先随便选，后面还可以更改）


# hexo环境搭建
官网才是正宗:  https://hexo.io/zh-cn/docs/

1、安装Git
安装完成后,记得测试一下:
```
	git verison 
 ```

2、安装Nodejs
`百度教程一大堆`
注意和官网要求的版本保持一致.
Notice: 一般安装完成nodejs后,就会带npm的.

3、安装hexo
使用npm安装Hexo 
```
	npm install -g hexo
```
`-g 是指全局安装`

4、安装完成之后,测试hexo是否安装成功. 

```
hexo -v
```

5、初始化hexo文件夹：
```
hexo init 
```
{% asset_img 2.png 初始化hexo文件夹}
注意执行此命令的时候,当前所处的目录.
完成后显示Start blogging with Hexo这串提示就说明安装成功啦

6、输入 `npm install` ，安装所需要的组件。此组件均安装在node_modules 文件夹中。
此时，本地的博客已经初步搭建完成。 
输入： `hexo s` 或者 `hexo server` 在本地启动服务,在4000端口.
打开浏览器访问: localhost:4000 即可看到效果.
![enter description here](https://foxgrin.github.io/img/Hexo/20.png)



##  npm 安装太慢解决
我就知道各位使用npm install的时候,会遇到没反应的情况.哈哈.

这个主要是由于npm安装是去国外源下载,由于某种原因,GFW墙了,所以我们需要想点办法去获得我们需要安装的文件. 
最简单的办法: 类似Docker配置国内源的方式,将npm的安装源,替换成国内的. 别急,替换国内的源: 
```
npm config get registry
```
不出所料,返回的应该是 http://registry.npmjs.org
 替换成国内阿里云
```
npm config set registry http://registry.npm.taobao.org
```
再试一下,飞一般的感觉哦~~~


# hexo 和GitHub pages关联
打开我们hexo博客的根目录.找到 `_config.yml`配置文件,打开.
![enter description here](https://foxgrin.github.io/img/Hexo/16.png)

滑到最下方,找到deploy: 
![enter description here](https://foxgrin.github.io/img/Hexo/17.png)

其中themes是主题,后面更换主题会用到
修改部署的:
![enter description here](https://foxgrin.github.io/img/Hexo/18.png)

注意所有的配置项,冒号后面都有空格.

```
hexo clean 
hexo generate 或者 hexo g
hexo deploy 或者 hexo d
```
会自动将我们的代码部署到github.io网站上.


至此,我们的博客基本已经搭建完成,下一节,讲解自定义主题.