---
title: 博客撰写技巧-markdown 如何首行缩进
date: 2019-05-26 23:46:01
categories: 博客
tags: markdown
---
## 问题与解决办法
**Markdown没有首行缩进**, 或许和它推崇"浸式的写作, 样式和内容分开"有关.我们要实现首行缩进, 就得从后期解析渲染入手.  
Markdown`使用易读易写的纯文本格式编写文档，然后转换成有效的XHTML（或者HTML）文档`[^1],它最后要给人看, 还是得转化成XHTML（或者HTML）文档的.因而**Markdown 完全兼容 HTML 语法，可以直接在 Markdown 文档中插入 HTML 内容**.  
### 方法一 段首插入HTML空格标记
所以解决办法很简单, **直接在段落开头插入html的空格标记就行了**.但是空格标记有好几种, 这里列举几种, 大家酌情选择:  
```
&nbsp;  不换行空格(No-Break Space), 和我们按下空格键(space键)产生的空格一样.宽度在不同的环境可能不同.  
&ensp;  半角空格(En Space), 占据的宽度是1/2个中文汉字宽度, 基本不受字体影响.
&emsp;  全角空格(Em Space), 占据的宽度是1个中文汉字宽度, 基本不受字体影响.  
说明: En 和 Em 是字体排印的计量单位, en 是 em 的一半, 1em 在16px的字体中就是16px.  
```
**注意**:空格标记包含了"**;* *"   
考虑到大家使用缩进都是中文环境, 所以建议在段落开始使用2个`&emsp;`来缩进, 方便且稳定.  
**方法**: 段首插入两个`&emsp`  
**优劣**: 每次都要插入, 有点麻烦, 但是这个方法比较稳定.  
### 方法二 使用全角空格
还有一种方法是在中文输入法下, 切换到全角输入. 一般是快捷键`shift+space`(标志是输入法上面有一个圆形和弯月形图案的变化,圆形是全角), 如果快捷键不管用, 可以直接去输入法设置里面去改.  
全角输入下, 按下空格键输入的全角空格, 所以直接按下空格键就可以实现段首缩进.  
**注意**: 这种方法在第一段的时候不管用, 在第二段之后才开始生效.   
**方法**: 全角输入下, 直接按下空格键   
**优劣**: 第一段不管用; 但其它段落方便, 且符合以前的使用习惯.如果不嫌麻烦, 可以第一段使用方法一, 之后的段落使用方法二.   

## 引申

1. 通过方法一, 我们可以在文章里面插入多个空格, 这样就解决了markdown里面多个连续空格只显示一个的问题(其实这和html一样, 空格不累加)
2. 英文书写习惯: 缩进还是不缩进?  
其时都行, 主要有两种标准的书写习惯:
`齐头式(blocked)`: 不缩进, 但是段与段之间空一行.  
`缩进式(indented)`: 段首缩进5个英文(英文输入法下5个空格键), 段落之间不空行.
---
## 参考文献  
[^1]: Markdwon 1.0.1 readme source code [Daring Fireball - Markdown](https://daringfireball.net/projects/markdown/)  
[^2]: [HTML中的\&nbsp;\&ensp;\&emsp;等6种空格标记](https://yunlian0621.iteye.com/blog/2360958)