---
title: Hexo 博客备份与恢复
date: 2019-05-29 10:43:35
categories: 博客
tags: 备份恢复
---
如果你搭建了自己的博客, 那就不得不考虑备份与恢复的事情。因为如果你换电脑或者重装系统，一旦原来的博客书写环境换了，重新搭建将是很麻烦的一件事。那些你为自己个性化选择的配置，各个平台上的服务集成，且不说你还记不记得怎么操作，就算按着教程，也得花费大把的时间。还好，我们有Github！接下来，我将介绍下如何结合Github来备份和恢复你的博客。  

## 说明
这里的前提是你已经搭建好了自己的博客，所以默认你已经安装配置好了git和Github环境；同时，Hexo博客也已经本地搭建配置好了。

## 备份  

我是将自己的博客挂载在Github Page上面，已经有一个同名仓库用来存放自己的站点静态文件（但是没有博客环境配置，因此还需要备份），所以没必要新建一个仓库来存放博客备份，只需要在这个仓库建立一个分支即可。为了简便，其实可以另建一个仓库来进行。


1. 首先克隆远程仓库

```
$ git clone git@github.com:yourGithubID/yourGithubID.github.io.git
```

2. 创建一个空白分支并删除原来的文件  

```
$ cd yourGithubID.github.io
$ git checkout --ophan BackUp #创建新分支BackUp
$ git rm -rf . # 删除所有文件
```

3. 将原有博客的内容全部拷贝到这个文件夹，如果是手动复制，请注意隐藏文件夹和文件  

```
$ cp -a yourBlogPath/. ./ # 注意是用.而非*, 如果是*的话， 无法复制隐藏文件和文件夹
```

4. 删除主题目录下.git(./themes/next/.git)文件，否则后面push到github上面时候会出错，如果喜欢折腾的话也可以将主题文件夹用[子模块](https://git-scm.com/book/zh/v1/Git-%E5%B7%A5%E5%85%B7-%E5%AD%90%E6%A8%A1%E5%9D%97)的方式添加;

```
$ rm -rf themes/next/.git
```
5. 编辑.gitignore文件  

Hexo站点文件以及是否需要备份说明如下:

```yml
    _config.yml站点的配置文件，需要拷贝；
    themes/主题文件夹，需要拷贝；
    source 博客文章的 .md 文件，需要拷贝；
    scaffolds/ 文章的模板，需要拷贝；
    package.json 安装包的名称，需要拷贝；
    .gitignore 限定在 push 时哪些文件可以忽略，需要拷贝；
    .git/ 主题和站点都有，标志这是一个 git 项目，不需要拷贝；
    node_modules/ 是安装包的目录，在执行 npm install 的时候会重新生成，不需要拷贝；
    public 是 hexo g 生成的静态网页，不需要拷贝；
    .deploy_git 同上，hexo g 也会生成，不需要拷贝；
    db.json文件，不需要拷贝。
```

因此, 编辑.gitignore文件(没有的话自己建立一个), 将不需要备份的文件屏蔽, 建议的文件内容如下:    

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

6. 备份

```
$ git add .
$ git commit -m "1st BackUp" #本地提交
$ git push origin BackUp # 提交到github 仓库
```
这样, github上面就有了一个干净的备份分支`BackUp`。接下来,去Github上面, 将默认分支设置成`BackUp`: **Setting -> Branches -> Default branch -> update**, 这样有助于后面恢复博客.  

这样下次备份的时候就更加方便, 只要执行下面指令

```
$ git add .
$ git commit -m "Messages you want to commit"
$ git push origin BackUp
```

## 恢复  

1. 安装git并配置
2. 安装Nodejs和npm

3. 克隆项目到本地

```
$ git clone git@github.com:yourGithubID/yourGithubID.github.io.git
```

4. 在克隆的文件夹下面, 输入如下命令来恢复博客:

```
$ npm install
```

因为我们备份了`packadge.json`文件，上诉命令会直接安装里面`dependencies` 和`DevDependecies`里面的插件， 安装完之后可以试运行看看是否备份成功。如果提示`hexo command not found` ，可单独安装hexo，`npm install -g hexo`.  
不需要这行`hexo init`指令, 因为并不是从零开始搭建博客.

---

## 参考文献
1. [Hexo 博客备份](https://blog.itswincer.com/posts/7efd2818/)  
2. [在Github上备份Hexo博客](https://lrscy.github.io/2018/01/26/Hexo-Github-Backup/)  
3. [git 创建一个空分支](https://blog.csdn.net/zs634134578/article/details/9183705)
4. [Github中创建新分支](https://blog.csdn.net/top_code/article/details/51931916)
