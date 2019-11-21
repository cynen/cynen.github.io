---
title: hexo构建博客(二)
date: 2019-11-21 23:31:47
categories:
  - 兴趣爱好
  - 个人博客
tags:
  - hexo
---

本节主要是介绍hexo更换主题
以及在搭建hexo期间容易产生的问题.

# Hexo 更换主题

经过上面的各种环境搭建，我们的博客可算是基本成形了，接下来就是对博客进行装修，选择一个我们喜欢的主题

我们可以在hexo官方上选择我们喜欢的主题（PS：[选择博客主题链接](https://hexo.io/themes/)）

选择自己喜欢的主题后点击主题名进入发布人的github

这里以我的博客选择的主题为例
![选取的博客](/images/2019/11/21.png)

进入对应的GitHub项目之后,获取对应的项目下载链接.

复制主题链接

然后回到blog目录下，找到并进入theme文件夹

右键选择git bash here

输入git clone +你选择的主题链接

下载所选主题
```angular2html
git clone xxx@ ..   themes/tomotoes
```
![](https://foxgrin.github.io/img/Hexo/23.png)

然后可以将主题文件夹名修改成较为简便好记的名字，这里修改成tomotoes

然后在blog目录下的_config.yml配置文件中，也就是刚才说的theme配置

修改成所选的主题名

![](https://foxgrin.github.io/img/Hexo/24.png)

## GitHub下载GitHub代码库特别慢

在下载tomotoes这个主题项目的时候,可能动不动遇到比较卡的情况,一个小项目clone到信心崩溃.

教一个特别骚气的招数: `忘记是在哪里看到的,作者看到,可以联系我`

1.将我们需要clone的GitHub代码,fork到自己的仓库中. 
2.登录国内一些代码平台,例如 [码云](https://gitee.com/) 
3.登陆后,将自己的GitHub代码库同步到码云.
4.本地直接clone码云上面的项目,速度快的不要不要的. [需要在码云注册.]

## 修改主题配置

在替换主题后,使用 `hexo s`启动后,预览,发现界面排版不正常.
![](/images/2019/11/25.png)

需要进入到对应的GitHub阅读文档.[也就是对应的主题提供的GitHub项目]

可见虽然主题更换了，但是一些css和js特效并没有显示出来，可能是这个主题需要特殊的一些配置文件，这时候我们就需要进入设计者的github主页中寻找接下来需要的设置

我们来到设计者的github页面，在主题下载链接下方会发现一个主题配置的要求
![](https://foxgrin.github.io/img/Hexo/26.png)

接下来就要仔细阅读这些要求，这里还是以我的主题为例

在Readme.md文件中有一些主题脚本或者css显示所需的配置安装
![](https://foxgrin.github.io/img/Hexo/27.png)

按照上面的配置命令逐一安装，这里就不一一说明了`这里需要安装好几个插件`

安装完成后再次运行hexo s

再来看看页面,发现已经恢复正常了.

## tomotoes配置

1.开启访问统计:
![访问统计](/images/2019/11/1.png)

参考: [不蒜子|不知](http://ibruce.info/2015/04/04/busuanzi/) 
按照说明配置,很简单.




# MD 简单总结
(1)文件开头：
``` 

---
title: xxx
tags: xxx
categories: xxx
description: xxx
date: 2018/7/12 22:00:00
---

```
(2)文章摘要：
`xxx<!--more-->
`

(3)图片插入：
`![](/img/1.png)`
（PS：在/blog/source目录下创建img文件夹，以后上传到文章的图片都保存在这里面）

(4)http链接插入：
`[内容](https://)`

(5)代码区：
` ``` code ``` `

(6)标题设置：
```
# 一阶标题 
##二阶标题 
###三阶标题 
####四阶标题 
#####五阶标题 
######六阶标题
```
这里md文件的编写，我强烈推荐使用Typora编辑器

最后，一切改动完成后，在blog目录下打开git，输入hexo d –g命令将改动更新到github上即可

# 问题及解决方

1.点击about或者左侧的分类,tags页面报错或者提示找不到.
解决办法:
1) 开启标签页:
```angular2html
hexo new page tags
```
执行完成会发现/blog/source下面多了tags文件夹，里面有一个index.md文件，在文件头内容中添加：

```angular2html
layout: tags
comments: false
```
以此类推,开其他页

开启关于页:
```angular2html
hexo new page about
```
在index.md文件中添加内容:
```angular2html
layout: about
comments: true
reward: false
```



# 参考链接
到这里,博客基本骨架搭建完成,后面只剩下自己写博客了.

[使用Hexo+github搭建属于自己的博客](https://Foxgrin.github.io/posts/29757/)
[hexo史上最全搭建教程](https://blog.csdn.net/sinat_37781304/article/details/82729029)





