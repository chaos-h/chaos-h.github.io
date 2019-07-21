---
title: hexo脚注插件hexo-footnotes解决不能插入脚注问题
date: 2019-05-27 15:27:00
categories: 博客
tags: 脚注
---
在写博客插入时候发现`[^脚注]`的语法不能用, 对比了网上的各种方法, 发现最简单的选择是安装[hexo-footnotes](https://github.com/LouisBarranqueiro/hexo-footnotes)插件.  
<!--more-->
**使用**: 在博客目录下运行下面这条命令即可.   

```yml
$ npm install --save hexo-footnotes
```

如果还是无效, 在站点配置文件` _config.yml`里面加入以下内容注册插件:   


```yml _config.yml
plugins:
  - hexo-footnotes
```
