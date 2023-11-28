
---
title: vscode+picgo搭建自己的图床
date: 2023-11-28 08:30:40
tags:
- vscode
- 图床
categories:
- 日常笔记
cover: https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011145928.png
---

# vscode + picgo 搭建自己的个人知识库

重点: 
1.阿里云oss开权限.
2.vscode配置picgo插件.

## 阿里云oss

主要是需要获得以下几个关键项:

```json
{
  "accessKeyId": "", 
  "accessKeySecret": "",
  "bucket": "", // 存储空间名
  "area": "", // 存储区域, 就是地区(oss-)
  "path": "" // 自定义的子目录,就是bucket内部创建的目录.
}
```

这一部分比较简单,重点是创建bucket时候,注意,一定要配置公共读.

### 创建bucket :

![20231011150713](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011150713.png)

下面的bucket功能都可以不用开. 

这里可以获得: 
```json
{
    "bucket": "haonanqu", // 存储空间名
    "area": "oss-cn-shenzhen", // 存储区域, 就是地区
}
```

### 获取Accesskey

通过自己的头像,打开访问控制.

![20231011151017](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011151017.png)


创建专用RAM用户.

![20231011151151](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011151151.png)

创建用户:
![20231011151326](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011151326.png)

一定要开启`OpenApi调用访问`

这里创建完用户后,就可以获取到该用户的 AccesskeyID 和 AccessKeySecret

如果忘记了,可以点击用户名称,进入当前用的属性页面,创建AccessKEY
![20231011152343](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011152343.png)


给指定的用户授予OSS完全控制权限.
![20231011151506](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011151506.png)


![20231011152048](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011152048.png)





## vscode安装picgo

vscode Settings --> Extensions --> 搜索picgo

![20231011145928](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011145928.png)

安装即可

## vscode配置picgo插件
打开Settings 


![20231011150259](https://cynen.oss-cn-shenzhen.aliyuncs.com/picgo/20231011150259.png)

注意:  最容易忽视的点,就是 `Picgo>Pic Bed>Aliyun>Path` 配置项,一定要带 `/`
不然就上传到根目录了.

```json
{
    "accessKeyId": "KeyID", 
    "accessKeySecret": "Secret",
    "bucket": "demo", // 存储空间名
    "area": "oss-cn-shenzhen", // 存储区域, 就是地区(oss-)
    "path": "picgo/", // 自定义的子目录,就是bucket内部创建的目录.
    "bucket": "haonanqu", // 存储空间名
    "area": "oss-cn-shenzhen", // 存储区域, 就是地区
}
```

## 使用
直接在vscode 写文档的时候(markdown),使用快捷键上传图片,会自动粘贴上传图片的地址. 


系统| 从剪贴板上传图像 |从资源管理器上传图像 |从输入框上传图像
---|---|---|---
Windows / Unix |`Ctrl + Alt + u` |`Ctrl + Alt + e`|Ctrl + Alt + o
macOS |Cmd + Opt + u|Cmd + Opt + e|Cmd + Opt + o
