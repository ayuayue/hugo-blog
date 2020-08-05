---
title: "关于页更改并加入一些在线服务"
date: 2020-08-01T13:40:02+08:00
draft: false
tags: [about]
categories: [about]
---

### 更改关于页面

先前的关于页面比较单一,本打算用`hugo`做一个下拉的子菜单显示一些更多的信息,但是无奈查询了文档和谷歌都没查到很好的先例,因此就用`hugo`的单页概念将页面分了级别.

页面由原来的一页分为了`n`个子页,通过一个`a`链接进行访问,目前整合了`联系我`,`在线提供的服务`,`友情链接`三个板块,如有需要可以在加,新建一个`markdown`文件放到`about`目录就可以了

### 关于在线提供的服务

目前仅有两个可以使用的服务

1. **在线的邮件系统**,仅支持`qq`邮箱,可以免登录发送邮件,如需接受邮件需要使用`qq`邮箱的授权码进行登录,仅仅使用`session`进行用户的记录,用户退出或者`session`过期后就会自动清楚,不会保留用户的任何信息.如登录后邮件为空或者出现错误,请刷新页面后重试

   

2. **go tour** :这个`golang`官方的教程,但是对于国内用户来说,访问go官方的链接比较慢,所以自己在本地搭建了一个官方的教程来帮助学习`golang`

### 关于友情链接

目前仅加入了几个,如果有想交换链接的可以在联系我里找到我的任何一个联系方式与我联系,也可以使用在线的邮件系统给我发邮件,或者在友情链接页下与我留言

注意交换链接请备注好您的格式,格式如下;

>1. 网站名称跟地址
>2. 网站描述
>3. 已添加我的博客的链接地址(交换审核用)