---
title: shell Learning Notes
date: 2019-07-16 22:30:16
categories: 程序员基本修养
tags:
  - shell
  - 程序员基本修养
  - 编程
---
以前零星写过很多次shell脚本, 用来编辑实现一些东西, 并不是太熟悉, 每次都要临时查, 这次搬运抄录一些基本知识, 算是系统复习下, 并作为日后查找的一个cache.
## 写在前面
### shell 和 shell 脚本
这是两个东西!  
**shell**是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。Ken Thompson的sh是第一种Unix Shell，Windows Explorer是一个典型的图形界面Shell。  
**shell脚本**（shell script）,是一种为shell编写的脚本程序。业界所说的shell通常都是指shell脚本，由于习惯的原因，简洁起见，本文出现的“shell编程”都是指shell脚本编fuxi程，不是指开发shell自身（如Windows Explorer扩展开发）。  
### 脚本解释器
常用的解释器有基础的sh,bash,以及扩展的ksh,csh,zsh等.这里简单介绍下sh和bash.  
**sh** 即Bourne shell，POSIX（Portable Operating System Interface of Unix）标准的shell解释器，它的二进制文件路径通常是/bin/sh，由Bell Labs开发.  
**bash** 是Bourne shell的替代品，属GNU Project，二进制文件路径通常是/bin/bash。业界通常混用bash、sh、和shell，比如你会经常在招聘运维工程师的文案中见到：熟悉Linux Bash编程，精通Shell编程.  
本文讲的脚本编辑器是sh, 其它大同小异同,差别处请参看文档.  

### 和高级语言之间的比较
shell 比较简单, 处理一些备份文件、安装软件、下载数据之类的事情, 方便易用。
但是处理复杂事物, 或者操作的数据结构比较复杂, 建议使用高级语言。

## 编写运行
### 编写  
打开文本编辑器，新建一个文件，扩展名为sh，就可以开始写shell脚本了。
一般第一行都是置顶脚本需要使用的解释器， 用`#!`来约定, 这和写python的时候一样，如下
```
#! /bin/bash #使用bash作为解释器
#! /usr/bin/python #使用python作为解释器
```
### 注释
**单行注释**
```
# 这里的内容被注释掉了
```
单行以“#”开头会被解释器忽略，即被注释
**多行注释**
```
:<<EOF
注释内容...
注释内容...
注释内容...
EOF

# 上面的EOF可以换成其它符号
:<<!
注释内容...
注释内容...
注释内容...
!
```
**也可以通过用花括号将代码括起来的方式，将其定义成函数，不调用来达到与注释相同的目的**
### 运行  
运行shell脚本有两种方法，一种是作为可执行程序，一种是作为解释器的参数。
### 作为可执行程序  
```
$ chmod +x example.sh #赋予脚本执行权限
$ ./example.sh        #在当前目录下运行example.sh
```
**注意事项**:
- 第一行一定要指定正确的解释器
- ./example.sh 不能简写成example.sh，因为这个执行程序并没有放入环境变量里面，没有写进PATH，直接运行系统无法识别命令来源，而用./example.sh 可以高数系统执行命令就在当前目录。  


### 作为解释器参数  
直接运行解释器， 其参数就是shell脚本的文件名， 如：
```
$ /bin/sh example.sh #其实可以直接sh example.sh,因为/bin/一般都在PATH里面
```
这种方式运行的脚本， 不需要在第一行指定解释器，事实上第一行在运行时被注释掉，不会发挥任何作用。

## 变量
```
idealLife="liberity"
echo $idealLife
idealLife="wealthy"
echo ${idealLife}
```
### 定义变量
```
变量名=变量
```
如上所示，变量定义用`=`, 并且**变量名和等号之间不能有空格**， 也可以用语句给变量赋值，如下：
```
for file in `ls /etc`
```
变量名无需申明，可以被重新赋值定义。
```
readonly 变量名 # 对已经定义的变量，用readonly声明可以将之声明为只读，不可被更改
unset 变量名 # 删除定义的变量，只读变量不可被unset删除
```
### 使用变量
```
$变量名   #方法一
${变量名} #方法二
```
使用变量有两种方法，在变量名前加`$`即可，花括号`{}`可加可不加，但是有的时候为了避免歧义，必须加，如下：
```
for skill in Ada Coffe Action Java; do
	echo "I am good at ${skill}Script"
done
```

## 字符串
### 单引号和双引号
**单引号**
```
str=`this is a string`
```
单引号的限制：
- 单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的
- 单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。
**双引号**  
```
your_name='chaos'
str="Hello, I know your are \"$your_name\"! \n"
```
- 双引号里可以有变量Unix
- 双引号里可以出现转义字符  


### 简单操作
**字符串拼接**
```
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```
输出结果为：
```
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```
**获取字符串长度**  
```
string="abcd"
echo ${#string} #输出 4
```
**提取子字符串**  
```
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```
## 数组  
sh持一维数组（不支持多维数组），并且没有限定数组的大小。
类似于 C 语言，数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。
### 定义数组
```
数组名=(值1 值2 ... 值n)
```
实例
```
array_name=(value0 value1 value2 value3)
# 或者
array_name=(
value0
value1
value2
value3
)
```
还可以单独定义数组的各个分量：
```
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen #可以不使用连续的下标，而且下标的范围没有限制。
```
### 读取数组
```
${数组名[下标]}
```
使用 @ 符号可以获取数组中的所有元素，例如：
```
echo ${array_name[@]}
```
### 获取数组的长度  
```
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```
## 传递参数  
我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。n 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……
```
#!/bin/bash
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```
执行：
```
$ chmod +x test.sh
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```
## 运算符
- 算数运算符：+,-,\*(需要转义),/,%,==,=,!=
- 关系运算符：-eq,-ne,-gt,-lt,-ge,-le
- 布尔运算符: !,-o,-a
- 逻辑运算符：&&,||（比布尔多个中括号）
- 字符串运算符: =,!=,-z,-n,$
- 文件测试运算符:-b,-c,-d,-f,-g,-k,-p,-u,-r,-w,-x,-s,-e

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。
```
#!/bin/bash
val=`expr 2 + 2` #反引号，运算符之间有空格
echo "两数之和为 : $val"
```
## 几个命令
### echo命令
echo 用于字符串的输出，命令格式如下：
```
echo string
```
具体可以更复杂的用法：
```
echo "It is a test" #显示普通字符串，双引号可以省略
echo "\"It is a test\"" #显示转义字符串
read name #read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量
echo "$name It is a test" # 显示变量num1="ru1noob"
num2="runoob"
if test $num1 = $num2
then
    echo '两个字符串相等!'
else
    echo '两个字符串不相等!'
fi
echo -e "OK! \n" # -e 开启转义，显示换行
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test" > myfile #显示结果定向至文件
echo '$name\"' #原样输出字符串，不进行转义或取变量(用单引号)
echo `date` # 显示命令执行结果，反引号
```
### printf命令
printf 由 POSIX 标准所定义，因此使用 printf 的脚本比使用 echo 移植性好。
**默认 printf 不会像 echo 自动添加换行符，我们可以手动添加\\n。**
printf 使用引用文本或空格分隔的参数，外面可以在 printf 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。
```
printf formatString [arguments...]
```
例子
```
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 # 与c类似的格式化规则
# 可以单引号，无引号
printf '%d %s\n' 1 "abc"
printf %s abcdef
# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def
printf "%s\n" abc def
# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n"
```

### test命令
Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。
```
test 包含三种运算符的语句
```
例子如下：

```
# 数值
## 其中[]配合$执行基本的算数运算，可以认为替代expr
num1=100
num2=100
if test $[num1] -eq $[num2]
then
    echo '两个数相等！'
else
    echo '两个数不相等！'
fi

# 字符串
num1="ru1noob"
num2="runoob"
if test $num1 = $num2
then
    echo '两个字符串相等!'
else
    echo '两个字符串不相等!'
fi
# 文件
cd /bin
if test -e ./bash
then
    echo '文件已存在!'
else
    echo '文件不存在!'
fi
```
## 流程控制  
**和Java、PHP等语言不一样，sh的流程控制不可为空，如果分支没有执行语句，那就不要写**  
### 判断
#### if
if 语法格式
```
if condition
then
    command1
    command2
    ...
    commandN
fi
```
写成一行用`；`隔开，适用于终端，如下示例，：
```
if [ $(ps -ef | grep -c "ssh") -gt 1 ]; then echo "true"; fi
```
if-else 语法格式
```
if condition
then
    command1
    command2
    ...
    commandN
else
    command
fi
```
if else-if else 语法格式
```
if condition1
then
    command1
elif condition2
then
    command2
else
    commandN
fi
```

示例
```
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi

## if else语句经常与test命令结合使用
num1=$[2*3]
num2=$[1+5]
if test $[num1] -eq $[num2]
then
    echo '两个数字相等!'
else
    echo '两个数字不相等!'
fi

```
#### case
```
case 值 in
模式1)
    command1
    command2
    ...
    commandN
    ;;
模式2）
    command1
    command2
    ...
    commandN
    ;;
    ....
    *)
        command1
    command2
    ...
    commandN
    ;;
esac
```
case工作方式如上所示。取值后面必须为单词in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;。
取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号\* 捕获该值，再执行后面的命令。

### 循环
#### for 循环
一般格式：
```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```
一行格式：
```
for var in item1 item2 ... itemN; do command1; command2… done;
```
#### while 循环
一般格式：
```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

#### 无限循环
```
while :
do
    command
done

## 也可以下面这样
while true
do
    command
done

##或者
for (( ; ; ))
```
#### until 循环
```
until condition
do
    command
done
```
#### break continue  
与其它高级语言一致 **break** 跳出循环，**continue** 直接开始下一循环。

## 函数   
函数格式：
```
[ function ] funcname [()]

{

    action;

    [return int;]

}

```
**说明**：
- 可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
- 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255
- 所有函数在使用前必须定义。

示例
```

funWithReturn(){
    echo "这个函数会对输入的两个数字进行相加运算..."
    echo "输入第一个数字: "
    read aNum
    echo "输入第二个数字: "
    read anotherNum
    echo "两个数字分别为 $aNum 和 $anotherNum !"
    return $(($aNum+$anotherNum))
}
funWithReturn
echo "输入的两个数字之和为 $? !"
```

### 参数传递
在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...
带参数的函数示例：
```
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```
**注意**，$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。
## 输入输出重定向
大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回​​到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。
重定向命令列表如下：  


| 命令                        | 说明                                                                              |
| --------------------------- | --------------------------------------------------------------------------------- |
| command > file              | 将输出重定向到 file                                                               |
| command < file              | 将输入重定向到 file                                                               |
| n > file                    | 将文件描述符为 n 的文件重定向到 file                                              |
| n >> file                   | 将文件描述符为 n 的文件以追加的方式重定向到 file                                  |
| n >& m                      | 将输出文件 m 和 n 合并。                                                          |
| n <& m                      | 将输入文件 m 和 n 合并                                                            |
| << tag                      | 将开始标记 tag 和结束标记 tag 之间的内容作为输入                                  |
| command1 < infile > outfile | 同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中 |


**注意**:需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。

### Here Document
ere Document 是 Shell 中的一种特殊的重定向方式，用来将输入重定向到一个交互式 Shell 脚本或程序。
它的基本的形式如下：
```
command << delimiter
    document
delimiter
```
它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。
**注意：**
- 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
- 开始的delimiter前后的空格会被忽略掉。

示例
```
$ wc -l << EOF
    非常感谢
    菜鸟教程
    www.runoob.com
EOF
3          # 输出结果为 3 行
```
### /dev/null
如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：
```
$ command > /dev/null
## 屏蔽 stdout 和 stderr
$ command > /dev/null 2>&1
```
/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。
## 文件包含  
Shell 文件包含的语法格式如下：
```
 filename   # 注意点号(.)和文件名中间有一空格
# 或
source filename
```

---
## 参考文献
1. [Shell脚本编程30分钟入门](https://github.com/qinjx/30min_guides/blob/master/shell.md)  
2. [Shell 教程.菜鸟教程](https://www.runoob.com/linux/linux-shell.html)    
3. [Advanced Bash-Scripting Guide](http://tldp.org/LDP/abs/html/)  
4. [Unix Shell Programming](http://www.tutorialspoint.com/unix/unix-shell.htm)  
5. [Linux Shell Scripting Tutorial - A Beginner's handbook](https://bash.cyberciti.biz/guide/Main_Page)  
