---
title: 记一次Hexo FATAL unknown block tag 问题解决
date: 2019-07-22 19:57:55
categories: hexo
tags: 
	- Hexo Render Error  
	- Unknown Block Tag 
---

一直在linux上面写博客，现在换mac，在备份博客后，运行博客`hexo g`预览, 发现如下问题提醒：
```
FATAL Something's wrong. Maybe you can find the solution here: https://hexo.io/docs/troubleshooting.html Nunjucks Error:  [Line 172, Column 4] unknown block tag: note 
```
提醒我是`unknown block tag: note`，即有未知的标签`note`, 这个不是hexo默认的可用标签，是`next`主题的拓展标签。于是我检查了下hexo主目录下的`themes\next` ，发现`next\`文件夹为空，因此报错。当我解决备份`next`主题内容的问题后，上面的`fatal unknow block tag`问题也得到解决了。  
**另**：像这种渲染的错误，都是.md文件内容上面的出错，只要找到出错的源头，自然也就能够解决这类问题。