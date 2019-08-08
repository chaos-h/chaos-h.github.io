---
title: linuxCommandScreen
date: 2019-08-06 13:17:17
categories:
    - a command a day
tags:
    - linux command
    - daily skills
---
{% cq %}
程序跑了一天，因为断网，连接中断，之前的运行全白费了！！！  
{% endcq %}

教训两则：

- 长期任务，最好后台运行，避免出现这种情况  
- 写代码，得鲁棒，懂得日志和周期性保存上下文的重要性  

对于第一则，就可以用今日份命令来解决！  

## 让程序后台运行的几种方式

### 后台运行命令&

在linux中，要让进程在后台运行，只要在命令后面加上`&`。
这其实是将命令放到一个作业队列中：

```python test.py
import time # time is the only thing I want to import for you
day = 1 # sadly day can only be one
love = 520 # they all say 520 means love
live = 1 # god shall tell me in this life
while live and love != 1314: # here I am and there shall be you beside
    time.sleep(day) # we get old anyway
    love += day # yet the love grows day by day
```

```python test.py
import time
day = 1
love = 520
live = 1
while live and love != 1314:
    time.sleep(day)
    love += day
```

```bash
$ python test.py &
$ jobs
[1]+ Running                    python test.py &
```

### 已经在前台执行的命令，重新放到后台执行；后台执行的程序，放回前台

`ctrl + z` 可以暂停运行的进程，使用`bg`命令将停止的作业放到后台执行，如：`bg %1`;
要放回前台：`%1`。

```bash
$ python test.py
^Z#(ctrl + z) # 暂停运行的进程
[1+] Stopped                     python test.py
$ bg %1  #放入后台
[1]+ python test.py &
$ %1 # 放回前台
python test.py
```

### nohup 和 setsid 和 subshell方式执行

**注意**: 前面的方法虽然可以将进程放到后台执行，但是其父进程还是当前终端shell进程。因此，一旦父进程退出(关闭shell等操作)，会发送`hangup`信号给所有子进程，子进程收到`hangup`以后也会推出。  
如果我们想要退出shell继续运行进程，则需要使用`nohup`忽略`hangup`信号，或者`setid`将父进程设为`init`进程(进程号1)，也可以通过`()`打开`subshell执行`：

```bash
$ nohup python test.py &
[1] 20172 # 这是这个进程的进程号
$ nohup: ignoring input and appending output to 'nohup'
$ setsid python test.py & #setid 执行，关闭shell 仍然运行
<关闭当前shell，并重开一个shell>
$ ps -ef| grep test.py #查找其他用户（shell）的进程
seainmli     67039     1  0 23:44 pts/9    00:00:00 python test.py

<关闭当前shell，并重开一个shell>
$ (python test.py &) # 在subshell 中后台运行，关闭shell仍然运行
$ ps -ef | grep test.py
seainmli     23455     1  0 23:56 pts/9    00:00:00 python test.py
```

说明：nohup 的用途就是让提交的命令忽略 hangup 信号，标准输出和标准错误缺省会被重定向到 nohup.out 文件中。一般我们可在结尾加上"&"来将命令同时放入后台运行，也可用" > log.out 2>&1"来更改缺省的重定向文件名。

### 对于已经在后台运行的程序，让它不受关闭shell影响

```bash
$ jobs
[1]+ Running   python test.py &
$ disown %1 # 此时关闭shell也不会停止
```

## screen

Screen是一款由GNU计划开发的用于命令行终端切换的自由软件。用户可以通过该软件同时连接多个本地或远程的命令行会话，并在其间自由切换，可以看作是窗口管理器的命令行界面版本。它提供了统一的管理多个会话的界面和相应的功能。

### screen 功能

- 会话恢复
  只要Screen本身没有终止，在其内部运行的会话都可以恢复。这一点对于远程登录的用户特别有用——即使网络连接中断，用户也不会失去对已经打开的命令行会话的控制。只要再次登录到主机上执行screen -r就可以恢复会话的运行。同样在暂时离开的时候，也可以执行分离命令detach，在保证里面的程序正常运行的情况下让Screen挂起（切换到后台）。
- 多窗口
  在Screen环境下，所有的会话都独立的运行，并拥有各自的编号、输入、输出和窗口缓存。用户可以通过快捷键在不同的窗口下切换，并可以自由的重定向各个窗口的输入和输出。Screen实现了基本的文本操作，如复制粘贴等；还提供了类似滚动条的功能，可以查看窗口状况的历史记录。窗口还可以被分区和命名，还可以监视后台窗口的活动。
- 会话共享
  creen可以让一个或多个用户从不同终端多次登录一个会话，并共享会话的所有特性（比如可以看到完全相同的输出）。它同时提供了窗口访问权限的机制，可以对窗口进行密码保护。

### screen 语法

```bash
screen [-AmRvx -ls -wipe][-d <作业名称>][-h <行数>][-r <作业名称>][-s ][-S <作业名称>]
```

参数说明：
    - -A 　将所有的视窗都调整为目前终端机的大小。
    - -d <作业名称> 　将指定的screen作业离线。
    - -h <行数> 　指定视窗的缓冲区行数。
    - -m 　即使目前已在作业中的screen作业，仍强制建立新的screen作业。
    - -r <作业名称> 　恢复离线的screen作业。
    - -R 　先试图恢复离线的作业。若找不到离线的作业，即建立新的screen作业。
    - -s 　指定建立新视窗时，所要执行的shell。
    - -S <作业名称> 　指定screen作业的名称。
    - -v 　显示版本信息。
    - -x 　恢复之前离线的screen作业。
    - -ls或--list 　显示目前所有的screen作业。
    - -wipe 　检查目前所有的screen作业，并删除已经无法使用的screen作业。

### 常用screen参数

screen -S yourname -> 新建一个叫yourname的session
screen -ls -> 列出当前所有的session
screen -r yourname -> 回到yourname这个session
screen -d yourname -> 远程detach某个session
screen -d -r yourname -> 结束当前session并回到yourname这个sessi

**在每个screen session 下，所有命令都以 ctrl+a(C-a) 开始**。  

|      命令      | 作用                                                               |
|:--------------:|:-------------------------------------------------------------------|
|     C-a ?      | 显示所有键绑定信息                                                 |
|     C-a c      | 创建一个新的运行shell的窗口并切换到该窗口                          |
|     C-a n      | Next，切换到下一个 window                                           |
|     C-a p      | Previous，切换到前一个 window                                       |
|    C-a 0..9    | 切换到第 0..9 个 window                                            |
| Ctrl+a [Space] | 由视窗0循序切换到视窗9                                             |
|    C-a C-a     | 在两个最近使用的 window 间切换                                     |
|     C-a x      | 锁住当前的 window，需用用户密码解锁                                 |
|     C-a d      | detach，暂时离开当前session                                         |
|     C-a z      | 把当前session放到后台执行，用 shell 的 fg 命令则可回去。             |
|     C-a w      | 显示所有窗口列表                                                   |
|    C-a t -     | Time，显示当前时间，和系统的 load                                    |
|     C-a k      | kill window，强行关闭当前的 window                                  |
|     C-a [      | 进入 copy mode，在 copy mode 下可以回滚、搜索、复制就像用使用 vi 一样 |
|                | C-b Backward，PageUp                                                |
|                | C-f Forward，PageDown                                               |
|                | H(大写) High，将光标移至左上角                                      |
|                | L Low，将光标移至左下角                                             |
|                | 0 移到行首                                                         |
|                | $ 行末                                                             |
|                | w forward one word，以字为单位往前移                                |
|                | b backward one word，以字为单位往后移                               |
|                | Space 第一次按为标记区起点，第二次按为终点                          |
|                | Esc 结束 copy mode                                                 |
|     C-a ]      | Paste，把刚刚在 copy mode 选定的内容贴上                            |
|                |                                                                    |

说明:C-a d 将目前的 screen session (可能含有多个 windows) 丢到后台执行，并会回到还没进 screen 时的状态，此时在 screen session 里，每个 window 内运行的 process (无论是前台/后台)都在继续执行，即使 logout 也不影响。

### 使用screen-常用

#### 安装

一般linux系统都会自带screen，如果没有安装的话，需手动安装

```bash
sudo apt install screen # ubuntu 18.04
# sudo yum install screen # others try this
```

#### 新建

```bash
# 方法一
screen # 新建一个窗口并进入，窗口无名字，不好区分，不建议

# 方法二
screen -S name # 新建一个名为name的窗口并进入

# 方法三
screen command # 新建一个窗口并在其中执行command命令，窗口无名字

```

#### 会话分离

screen的一个有点就是你可以退出当前窗口去做其它事情，让程序在后台运行：

```bash
# 方法一
C-a + d # 快捷键，会弹出[detached]的提示，并回到主窗口

# 方法二
screen -d name # 远程detach某个session，前提是已经跳出了name窗口

```

#### 会话恢复

```bash
$ screen -ls # 列出窗口列表
There is a screen on:
    42172.test      (Detached)
1 Sockets in /var/run/screen/S-seainmli.
$ screen -r test # or screen -r 42172
```

#### 杀死会话窗口

```bash
# 方法一
kill -9 threadnum # 通过杀死线程的方式 比如 kill -9 42172

# 方法二
exit   # 当前窗口退出登陆即可

# 方法三
C-a + k # 快捷键 ctrl+a+k 杀死当前窗口和窗口中运行的程序

# 方法四
C-a
quit # 这样退出会杀死所有窗口并退出其中运行的所有程序。

```

#### 杀死死去的会话

```bash
screen -wipe # 自动清处死去的窗口
```

---

### 参考文献

[1] [Linux 进程后台运行的几种方式](https://segmentfault.com/a/1190000002607962)  
[2] [linux screen 命令详解](https://www.cnblogs.com/mchina/archive/2013/01/30/2880680.html)  
[3] [GNU's Screen 官方站点](http://www.gnu.org/software/screen/)  
