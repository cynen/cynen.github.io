---
title: Docker搭建FastDFS
date: 2019-11-24 18:13:40
tags:
- docker
- fastdfs
categories:
- docker
- 应用部署
---


# 简介

此篇是docker部署fastdfs教程,针对具有一定的docker容器基础的.

# FastDFS架构:

![](/images/2019/11/4/1.jpeg)
服务端两个角色：
Tracker：管理集群，tracker 也可以实现集群。每个 tracker 节点地位平等。收集 Storage 集群的状态。
Storage：实际保存文件   Storage 分为多个组，每个组之间保存的文件是不同的。每个组内部可以有多个成员，组成员内部保存的内容是一样的，组成员的地位是一致的，没有主从的概念。


# 文件上传及下载的流程
## 文件上传流程
![](/images/2019/11/4/2.jpeg)
文件上传： 
客户端链接到Server之后，可以从Server端获取到Storage的ip和port，然后将指定的文件通过storage的服务端口上传。 【此处使用的是配置文件中的 port=23000 还是http.server_port=8888】上传完成后,回传对应的文件的id.
 

客户端上传文件后存储服务器将文件 ID 返回给客户端，此文件 ID 用于以后访问该文件的索引信息。文件索引信息包括：组名，虚拟磁盘路径，数据两级目录，文件名。
![](/images/2019/11/4/4.jpeg)
 
* 组名：[group1/M00]文件上传后所在的 storage 组名称，在文件上传成功后有 storage 服务器返回，需要客户端自行保存。
虚拟磁盘路径：storage 配置的虚拟路径，与磁盘选项 store_path*对应。如果配置了
store_path0 则是 M00，如果配置了 store_path1 则是 M01，以此类推。
* [02/44/]数据两级目录：storage 服务器在每个虚拟磁盘路径下创建的两级目录，用于存储数据
文件。
* [一长串]文件名：与文件上传时不同。是由存储服务器根据特定信息生成，文件名包含：源存储
服务器 IP 地址、文件创建时间戳、文件大小、随机数和文件拓展名等信息。

## 文件下载流程 
![](/images/2019/11/4/3.jpeg)
文件上传完成后,进行文件下载的时候,指定的是file_id. 
客户端链接到Server,获取到指定文件存储的文件信息,重定向请求到Storage服务器上,由于Storage服务器上无法提供文件服务,因此,此请求需要通过nginx做代理,将静态文件传递回给客户端.

# Docker部署FastDFS

OK,上面大致讲解了一下FastDFS的基础架构,现在准备来进行搭建吧.
由于直接在linux上进行搭建比较繁琐[具体搭建方法百度一下],因此,我这里采取我比较熟悉的Docker容器进行搭建.顺便记录一下搭建过程中遇到的一些问题和注意事项.

搭建参考: [docker搭建fastdfs](https://www.cnblogs.com/yanwanglol/p/9860202.html)

## 关键几步： 
### 1、使用docker镜像构建tracker
（跟踪服务器，起到调度的作用）：
```
docker run -d --network=host --name tracker -v /var/fdfs/tracker:/var/fdfs delron/fastdfs tracker
```
说明: 
`--network=host` 指定此容器运行使用的是host网络模式,也就是和宿主机共用物理网卡.占用的端口也就是物理机的端口.`非常重要!!`
`tracker` 最后一定要指定一下,启动的容器是 tracker还是storage.
`-v ` 目录映射,就是为了将容器内的目录,文件映射到宿主机上,方便修改.


### 2、使用docker镜像构建storage
实际的执行存储和读取文件的服务器
```
docker run -d --network=host --name storage \
-e TRACKER_SERVER=ip:22122 \
-v /var/fdfs/storage:/var/fdfs \
-e GROUP_NAME=group1 delron/fastdfs storage
```
`storage` 最后面的一个storage一定要指定一下.
`-e TRACKER_SERVER=ip:22122` 指定tracker服务器的地址和端口号. (所以,22122是可以换的)
`-e GROUP_NAME=group1`指定当前的组名称


### 3、部署完成后，对服务器进行配置 
【都是在storage容器内部操作】
 1、进入storage容器，到storage的配置文件中配置http访问的端口，配置文件在/etc/fdfs目录下的storage.conf。

`vim /etc/fdfs/storage.conf`
Storage 的服务端口：
![](/images/2019/11/4/5.png)

2、配置nginx，在/usr/local/nginx/conf目录下，修改nginx.conf文件
`cat /usr/local/nginx/conf/nginx.conf`

修改Nginx的配置： 【默认配置如下】

![](/images/2019/11/4/6.png)

可以这么修改： 【好像也可以不用配置。。。我使用的是默认配置.】
```
location /group1/M00 {
        alias  /var/fdfs;
}
```

安装完成后进行 测试：【storage容器内部操作】
由于已经将目录挂载到主机了。所以，我们直接给主机的挂载目录上传一张图片。 【这个。。。自己想办法吧,实在不行就 wget吧】
![](/images/2019/11/4/7.png)
这个是我传上去的一张照片，此时并没有被fastDFS管理！！！
上传指令：  【需要正确的1.jpg的路径。】
进入到/var/fdfs/目录下,查看是否存在图片文件.
在图片目录下执行一下命令:
```
/usr/bin/fdfs_upload_file /etc/fdfs/client.conf 1.jpg
```
上传成功后,会返回文件的id:
![](/images/2019/11/4/8.png)

OK，在浏览器，直接访问：
`http://{storage主机ip}:8888/group1/M00/00/00/rBAINlzw74qAWgPgAAS9dM2zyF4598.jpg`
即可看到对应的图片.


### Storage中的配置文件：
我们来看一下一共使用了很多端口: 
![](/images/2019/11/4/9.png)

【Storage容器内操作】
```
vim storage.conf

// 关注其中几个参数:   
// 这个其实是我们运行此容器的时候,指定的参数.
group_name=group1
// 这个端口是Storage的端口,应该是storage和Tracker通信的端口.很重要.
// 此端口可以修改,只要是唯一即可.修改成ECS在安全组里放行的端口.[必须是放行的端口.]
port=23000
// 指定基础路径,存放文件位置的.
base_path=/var/fdfs
// 扩容的路径
store_path0=/var/fdfs
//指定tracker服务器的地址,这个在容器运行时,做了指定,在这里也可以手工配置.
tracker_server=47.107.177.108:22122
// storage服务的端口,客户端从tracker获取到的storage的服务端口.[需要和客户端做通信的.非常关键.]
http.server_port=8888
// 此端口是storage和客户端进行通信的端口.修改了此端口,就需要修改 storage中的nginx监听端口.
// 
```

Nginx配置文件:
```
cd /usr/local/nginx/conf/
vim nginx.conf 

    server {
        // 指定监听端口,和storage的http.server_port端口一致.
        listen       8888; 
        server_name  localhost;
        // 指定地址.
        location ~/group[0-9]/ {
            ngx_fastdfs_module;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root html;
        }
    }

// =================== 如果不好使的话,这么替换: ==============
// 目前阿里云上,我没有配置,也可以直接访问了.
       // 指定地址.
       // 配置这个的目的,是为了浏览器能够直接访问到文件.
        location /group1/M00 {
            alias /var/fdfs; // storage指定的存储文件的位置.
        }
```


## 个人总结: 
* 上传时候,使用的应该是链接 tracker的22122端口, 从tracker获取storage的8888端口. `此8888端口是我们自己配置的`
* 客户端通过tracker实际是获取到对应storage服务的8888端口.
* 客户端既要和tracker通信`类似Zookeeper` 又要和storage通信.
* 对比Dubbo的架构设计,Zookeeper作为调度中心,客户端优先通过2181访问Zookeeper服务器,然后获得对应接口的远程服务器的ip:port然后调用远程服务器的服务. `很像吧...`


