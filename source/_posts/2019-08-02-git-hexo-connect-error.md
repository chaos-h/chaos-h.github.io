---
title: hexo部署-记一次github连接出错问题
date: 2019-08-02 21:28:39
categories: 
    - hexo
    - github
tags:
    - error
---

## 问题

在公司办公电脑上配置hexo博客环境，发现无法从github仓库clone自己的博客文件，提示错误如下：  

```bash
ssh: connect to host github.com port 22: Connection timed out
fata: could not read from remote repository
please make sure you have the correct access rights
```

## 分析

刚开始以为是权限问题，于是在github上重新上传了自己ssh公钥公钥，发现还是不行。
接着开始google，看到答案让“新建config文件，填入相关信息”，但是并不管用。
后来看到答案，将**连接方式由ssh转为http**得到解决。

## 解决

我们都知道github上面的仓库clone有`HTTPS`和`SSH`两种方式，`SSH`不行，`HTTPS` 便成了另一种选择。之前“更改config文件”的方式，也是将22号(ssh默认端口)改成443(HTTPS默认端口).  

```bash
# 使用如下命令替换git clone git@github.comxxx
git clone https://github.com/yourname/yourname.github.io.git
```

## hexo deploy 配置

通过上面可以将仓库`clone`到本地，`push`也没问题，但是`deploy`会报错，这是因为deploy上面的部署策略也要修改：

```yml _config.yml
# 配置如下即可
deploy:
  - type: git
    repository: https://github.com/yourname/yourname.github.io.git
    branch: master
```
