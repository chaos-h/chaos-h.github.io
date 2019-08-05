---
title: python2 input()、raw_input() 与 python3 的 input()
date: 2019-08-05 22:12:42
categories:
    - coding
    - python
tags:
    - python
    - Errors 
---

## python 2 input 和 raw_input的区别

1. input() 相当于赋值，会直接接受数字的数据类型，计算表达式的值

    ```python2
    >>> a = input("Please input your favorite number: ")
    Please input your favorite number: 5
    >>>a
    5
    >>> b = input("Try another number: ")
    >>> 5 + 3
    >>> b
    8
    ```

2. input() "NameError: name '***' is not defined" 问题

    ```python2
    >>> a = input("please input your name: ")
    please input your name: seainm
    Traceback (most recent call last):
        File "<stdin>", line 1, in <module>
        File "<stdin>", line 1, in <module>
    NameError: name 'seainm' is not defined
    ```

    如上面例子所示，我们想输入的其实字符串"seainm", 但是我们是直接输入`seainm`, 解释器把它当作一个变量，因此报`not defined`的错误。正确的输入应该是：

    ```python2
    please input your name: 'seainm'
    >>> a
    seainm
    ```

3. raw_input()字符串输入

    ```python2
    >>> a = raw_input("Please input your name: ")
    Please input your name: seainm
    >>> a
    seainm
    >>> b = raw_input("Please input your age: ")
    Please input your age: 19
    >>> b
    '19'
    ```

    从上面例子可以看出，与`input()`不同，`raw_input()`把输入的内容都当作了字符串。
4. 总结
    python2中，`input()`要求我们明确我们输入数据格式，是数字还是字符串；而`raw_input()`把所有的输入都当作字符串处理。

## python3中的input()

python3中`input()`其实就是python2中的`raw_input()`, 把所有输入都当作字符串。同时。  

---

## 参考文献

[1] [python2中的input()、raw_input()、和python3中的input()](https://www.cnblogs.com/gengcx/p/6707024.html)  
