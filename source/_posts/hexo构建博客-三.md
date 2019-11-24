---
title: hexo构建博客(三)
date: 2019-11-24 01:09:54
tags:
- hexo
categories:
- 兴趣爱好
- 个人博客
cover: /img/study.jpg
---

# ButterFly 主题的应用.

## Butterfly主题下载.

下载参照上一步的变更主题的链接: [hexo构建博客(二)](https://cynen.github.io/2019/11/21/hexo%E6%9E%84%E5%BB%BA%E5%8D%9A%E5%AE%A2-%E4%BA%8C/),直接搜索 Butterfly

![](/images/2019/11/3/1.png)

根据README文档,下载对应的主题. 
1.并且修改`_config.yml`配置文件,此配置文件是hexo的配置文件,注意和themes主题的配置文件区分. 
2. 安装对应的依赖.
![](/images/2019/11/3/2.png)

3.启动服务 `hexo s` 打开浏览器,输入  `localhost:4000` 即可看到效果.

参考: [roger博客](https://roger0917.github.io/about/)


## 自定义参数配置.

ButterFly 主题的文档有时候打不开,可以参考 [hexo-theme-melody](https://molunerfinn.com/hexo-theme-melody-doc/theme-config.html#highlight-theme)  ButterFly是基于此主题开发的.

打开 ButterFly 主题的配置文件.针对自定义的参数进行配置.

基本上看英文就知道是什么了,在此,重点关注几个地方:

1.language: 配置语言.  此配置项应该是在hexo的配置文件中配置. 
`目前配置完成后,貌似偶尔不生效.`
解决办法:将对应主题下的其他language文件全部删除,只留对应配置的语言文件即可解决此问题(不知道为啥).

确保对应的主题中的themes/languages 文件夹下有对应的文件.
![](/images/2019/11/3/3.png)

2.修改菜单为中文.
![](/images/2019/11/3/4.png)

3.页面动态效果配置:

需要的就将对应的置为: true即可.
```
canvas_ribbon: 彩带
canvas_ribbon_piao: 动态彩带
canvas_nest: 页面类似线条网状蒲公英
fireworks: 烟火特效,点击后烟花
click_heart: 点击时,弹出爱心
ClickShowText: 点击时,出现文字
```
简繁转化我关闭了.
![](/images/2019/11/3/5.png)

其他的可以自行摸索.

### 图片配置
在md头中,可加入此参数,配置每个md文件的cover[封面]
```
cover: https://api.dujin.org/pic/ 
// 以上是二次元的图源
```
此处建议尽量配置成单个图片的.
以下是微软的bing搜索墙纸
`https://uploadbeta.com/api/pictures/random/?key=BingEverydayWallpaperPicture`
如果不指定具体的图片,而是每次都重新读取,那么博客的图片也会每次都不一样.
但是这个会导致归档的时候,无法展示图片. 

## 搜索配置

我们可以看到,Butterfly给我们提供了2种搜索模式.

![](/images/2019/11/3/6.png)

由于第一种需要配置AppID等参数,我们暂时关闭掉.

开启第二种搜索. [只有开启搜索后,页面上才会有搜索框]
![](/images/2019/11/3/7.png)

点击搜索,报错.
![](/images/2019/11/3/8.png)

解决: 找了各种参数,都没有找到怎么解决,百度一下. 发现local_search居然是hexo自带的功能...

参考: 
[Hexo博客,搜索无效解决](https://www.jianshu.com/p/02afabcae502)
[Hexo开启站内搜索](https://www.jianshu.com/p/519b45730824)
额... 是因为需要安装插件的原因

```
npm install hexo-generator-search --save
npm install hexo-generator-searchdb --save
```

安装完毕后,再刷新页面.
![](/images/2019/11/3/9.png)

搜索可用!

## 开启评论功能

ButterFly 主题,提供了2中评论功能的插件.
一种是 [gitalk](https://www.jianshu.com/p/656e6101bf0f)
一种是 [valine]()

### gitalk的使用:
参考: 
[在个人博客添加评论](https://www.jianshu.com/p/656e6101bf0f)

貌似我配置完之后,没有效果.(有效果的给我反馈一下,我好取消我的learnCloud账号.)
![](/images/2019/11/3/10.png)

既然gitalk实现不了, 那就使用valine来实现吧.

### 配置 valine: 
![](/images/2019/11/3/11.png)
效果:
![](/images/2019/11/3/12.png)
OK,实现了评论功能!


使用valine注意事项:
`需要申请learnCloud账号,要支付宝实名认证!要支付宝实名认证!要支付宝实名认证!`

注册过程我就不详述了,自己按照步骤来即可的.

参考 : [learnCloud入口](https://valine.js.org/quickstart.html)



Hexo博客,从我突发神经想搞,到现在基本知道怎么整,前后差不多花了一周时间.

一抬头,发现现在都凌晨 2点了,好困好饿...我先去吃碗泡面,休息了,休息了.
