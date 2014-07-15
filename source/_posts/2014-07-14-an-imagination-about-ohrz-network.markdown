---
layout: post
title: "一个具有P2P+个人分享+代理功能网络的设想"
date: 2014-07-14 15:41:43 +0800
comments: true
categories: 
---

几天前，当我看着我挂机的电脑时，我产生了这样一个想法，当今网络已经十分发达，但实际上还有很大的利用空间。大量的路由器、带宽资源闲置；人们从少量的服务器上获取资源使得服务器压力大而且下载速度慢。BT/PT的存在改善了这种情况，但具有一些局限性还是不够普及。我设想用一种更易用而且可靠的网络使人们更有效地获取资源，名曰**Ohrz Network**，简称O网。


## 理想的功能

---

以下内容脑洞全开，不考虑如何实现

<!-- more -->

## O网（O Net）

---

### 官网
> 暂时用ohrz.me这个域名，真正做起来可以用ohrz.net
> （八字还没一撇纠结这个干嘛）

### O User
> O网账号，具有
>> 1. 个人资料: 用户名，注册时间（，关注列表？）
2. 个人共享文件或文件夹
3. P2P下载上传的总量记录，用来作为用户等级、anti-leech等

### O URL

> O网中的文件或文件夹有其对应的O URL，O URL以ohrz://开头，并支持以下几种格式：
>> 1. ohrz://|HASH|SIZE|FILENAME 或 ohrz://|HASH|SIZE|FOLDER 类似ed2k、magnet链接
2.  ohrz://OUSER_NAME/(FOLDER/)FILENAME 或 ohrz://OUSER_NAME/FOLDER 用户可以有自己的共享文件，并可以设置读取、写入权限的密码
3. ohrz://http/URL 或 ohrz://ftp/URL 兼容传统URL格式，使之可以被下载器检测到并将此URL及对应文件加入O网。用户还可以根椐URL在O网搜索相关文件，得到1或2中的O URL
4. ohrz://ext/[youku|tudou|baiduyun|mega]/(ID 或 URL) 通过扩展支持一些第三方服务 

> 注1: 若是文件夹，可以从O网中获取文件夹的目录结构及文件大小信息。文件夹可由密码保护

> 注2: O网具有足够多足够快的源使得O网中包括视频等大型文件可以在线预览

> 注3: 若格式1的链接未给出HASH甚至SIZE信息，将从O网查找最可能的文件


## 客户端（O Client）

---

### O Downloder

> 作为一个下载器，除了从传统URL直接下载文件，还具有以下功能：
>> 1. 下载O URL文件，从官方或镜像或P2P下载
2. 从O网获取一个URL对应文件的镜像URL，使用镜像站点加速下载
3. 从O网获取一个URL对应文件的peers列表，这可以说是BT/PT下载的核心。形像地说就是知道了已经下载完成和想下载这个文件的人，于是你们就可以互相帮助下载完这文件了
4. 从O网获取一个URL对应文件的介绍等相关信息
5. 从O网搜索文件名，得到O URL链接（类似eMule网络）

> 注1: 当O网中有一个URL及其对应文件的记录时，可以确定这个URL对应这个文件。若出现一个URL还没人下载完但有多人下载的情况，peers之间可以进行P2P加速但此文件在O网中没有hash值等信息直到有人下载结束

> 注2: 为保证O网中的文件及URL信息真实可靠，客户端必须发送真实正确的文件信息。为防止恶意破坏，客户端需加密数据传输并可能因此不开源

> 注3: 其实2、3迅雷已经有并且做了很多年了，这也是迅雷下载速度快的原因之一。除此以外，迅雷还建成了它的文件云，为用户提供下载加速。但是，迅雷是封闭、收费的，在P2P方面甚至可以用流氓来形容。迅雷N年前就开始在用户不知情的情况下在后台上传文件（界面上没有上传的迹像，也没有关闭的选项）给其他迅雷用户，甚至有扫描硬盘一说。同时迅雷也是BT、eMule中的吸血客户端（即只下载不上传），在各PT中都是被ban的。我只想说，净化P2P环境，从抵制迅雷做起

### O Server

* hosts local file with listing, file intro(, comments?)
* act as cache server, answering queries about file or URL

### O Proxy

* O网中的每个用户可以当作O CDN的一个节点，缓存网页中的静态文件（CDN从我做起）

* 如果需要，O网客户端可作为上网代理节点(天朝人民十分需要)

---

## 一些需要解决的问题

---

* load balance
* URL失效
* leech
* block
* file intro (who can add?)
* file comments (where to host comments on a P2P network?)
* **保证网络中充当服务器角色的数量应大于下载角色的数量**


## 可能的实现方法

---

* **a network without central server**
> 服务器只提供用户认证，文件相关不保存在服务器
* 用户缓存文件相关信息
* 用普通HTTP协议混淆
* 为同一学校（局域网）或城市提供channel
* 查询O网文件信息采用类似DNS系统的分级缓存，用户从最近的缓存节点进行查询


## 谁需要它？

* 天朝用户
* 校园网用户
>  对校园网的最终目标是：**无需建站和维护，自动为每个校园网（即大型局域网）建立等同于PT的分享网络**
* 所有用户

### 可以参考的开源项目

---

* libTorrent
* pyLoad
* shadowsocks 
* you-get
* obfsproxy