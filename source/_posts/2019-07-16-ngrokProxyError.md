---
title: ngrok Proxy Error 解决办法
date: 2019-07-16 15:19:09
categories: 日常玩机
tags: 内网穿透
---
## 前言
如果需要在局域网外访问没有公网ip的机器, 比如在校外ssh连接自己实验室的电脑, 就要用到内网穿透. 实现内网穿透有很多中方法, 诸如使用远程桌面工具, 路由器端口映射, 反向代理工具. 最终, 我选择的是[ngrok](https://ngrok.com/), 理由是**免费、易用**.  
## 问题描述    
在最初使用ngrok的时候, 出现了如下问题:  
```yml
 ERROR:  Invalid configuration property value for 'http_proxy', '127.0.0.1:1080': parse 127.0.0.1:1080: first path segment in URL cannot contain colon
```  

## 问题原因  
直接google, 没有得到解决办法, 只好自己分析解决.
看问题描述, 知道是**配置**出了问题, 且是 http 代理上出了问题, 是网址解析的时候出错, 不能包含冒号.
查看[ngrok文档](https://ngrok.com/docs), 发现在配置文件里面可以修改http代理配置, 作对应修改即可.根据问题提示, 以及自己的测试, 发现是语法的问题,我在系统里面的代理设置是没有带"http://"这个前缀的, 而nigrok的配置必须带前缀才能生效, 否则会报错.

## 问题解决
在配置文件里面修改http代理设置, 各个系统配置文件`ngrok.yml`的位置如下,将下面的*username*替换成你自己的用户名:  

```  
OSX    /Users/username/.ngrok2/ngrok.yml
Linux  /home/uername/.ngrok2/ngrok.yml
Windos C:\Users\example\.ngrok2\ngrok.yml
```  
在配置文件里面添加http代理配置  

```yml ngrok.yml
http_proxy: "http://127.0.0.1:1080"
```   
如果还是不行, 可以尝试将代理关闭:
```yml ngrok.yml
http_proxy: false
```  
## 后记
我出现这个问题是因为终端使用了代理, 而且是shadowsocks代理, 所以添加http代理的方式虽然能让ngrok运行, 却无法正常使用, 将http代理关闭却可以.
