---
title: linux 命令笔记
date: 2019-05-26 22:22:48
categories:
- linux
tags:
- linux命令
description: 记录日常用到的一些命令
---

> 记录日常用到的一些命令

### 命令行下安装.deb包及解决依赖问题   
有的时候需要命令行安装.deb软件包, 比如无法使用桌面环境的时候, 使用图形界面卡顿或者安装不了.这个时候就会用到*dpkg*命令.   
安装命令, 假设安装的包叫*teamviewer.deb*, 命令行输入如下命令即可:

```
$ sudo dpkg -i teamviewer.deb
```

但是**dpkg命令无法自动解决依赖关系, 如果安装的.deb包存在依赖包, 则会报错**  
这个时候, 运行下面命令, 先安装依赖即可:  

```
$ sudo apt install -f  
```

安装完依赖后再执行安装dpkg指令.  
补充([dpkg命令的其它使用](https://www.cnblogs.com/windtail/archive/2012/06/02/2623175.html)):  

```yml
sudo dpkg -I teamviewer.deb #查看软件包的详细信息，包括软件名称、版本以及大小等（其中-I等价于--info）
sudo dpkg -c teamviewer.deb #查看teamviewer.deb软件包中包含的文件结构（其中-c等价于--contents）
sudo dpkg -i teamviewer.deb #安装teamviewer.deb软件包（其中-i等价于--install）
sudo dpkg -l teamviewer.deb #查看teamviewer.deb软件包的信息（软件名称可通过dpkg -I命令查看，其中-l等价于--list）
sudo dpkg -L teamviewer.deb #查看teamviewer.deb软件包安装的所有文件（软件名称可通过dpkg -I命令查看，其中-L等价于--listfiles）
sudo dpkg -s teamviewer.deb #查看teamviewer.deb软件包的详细信息（软件名称可通过dpkg -I命令查看，其中-s等价于--status）
sudo dpkg -r teamviewer.deb #卸载teamviewer.deb软件包（软件名称可通过dpkg -I命令查看，其中-r等价于--remove）  
```
### linux 终端连续执行多条命令  
### 分号(;)
```
$ command0;command1;...;commandn
```
**作用**: 顺序执行多条命令
**执行顺序**: 顺序执行, 互不影响
### 逻辑与(&&)
```
$ command0&&command1&&...&&commandn
```
**作用**: 顺序执行多条的命令, 直到执行失败或结束
**执行顺序**: 前面的命令执行成功, 才会继续执行后面的命令
### 逻辑或(||)
```
$ command0||command1||...||commandn
```
**作用**: 顺序执行多条命令,直到成功执行或者结束
**执行顺序**: 前面的命令执行失败, 才会继续执行后面的命令;执行正确, 后面的命令不会执行
### 逻辑与或组合  
```
$ command0&&command1||command2
```
逻辑连词可以混合使用,如示例, command0执行成功,则执行command1,否则,执行command2.
### 管道(|)
```
$ command0|command1|...|commandn
```
**作用**: 顺序执行有输入输出依赖的多条命令
**执行顺序**: 前面的命令的输出作为后续命名的输入

---
### 参考文献
1. [bash执行多条命令](https://blog.csdn.net/zly9923218/article/details/53811724)  
