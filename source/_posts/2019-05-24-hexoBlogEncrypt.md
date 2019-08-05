---
title: hexo博客加密
date: 2019-05-24 11:15:14
categories:
    - 博客
tags:
    - 加密
---

## 问题描述

写博客是为了输出, 有的时候, 我们希望只有部分人能看到它. 如何做到呢?

## 问题解决  

对博客加密, 只有输入正确的密码才能查看博客.  
为了实现hexo博客的加密, 我们要用到博客加密插件[hexo-blog-encrypt](https://github.com/MikeCoder/hexo-blog-encrypt).  
具体使用如下:

1. 在站点目录下运行下面命令安装插件*hexo-blog-encrypt*  

    ```bash
    npm install --save hexo-blog-encrypt
    ```

2. 修改站点配置文件 *_config.yml* ,启用改插件, 在最后面加入如下内容  

    ```yml hexo/_config.yml
    #security  
    ##  
    encrypt:  
        enable: true
    ```

3. 使用  
使用非常简单, 只要在每篇post的*Front-matter*,也就是开头---隔开的配置部分加上你的密码和摘要内容即可, 如下面所示:

    ```yml
    ---
    title: hexo博客加密  
    date: 2019-05-24 21:18:02  
    password: 这里填写你的密码  
    abstract: 这里的内容是你的博客在首页展示的内容, 也就是没点进去之前大家都能看到的东西.  
    message: 这里的内容是点击博客链接, 博客展示的内容, 输入密码前大家可以看到的详细内容.  
    ---
    ```
