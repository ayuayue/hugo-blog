---
title: Markdown简易教程
date: 2020-05-16T22:24:08+08:00
tags: [Markdown]
categories: [Markdown]
---

#### 	什么是`Markdown`,为什么用`Markdown`

`Markdown`是一种标记语法,通过标记字符,给文章的内容增加样式,使用`Markdown`可以更方便的控制格式的同时专注于文章内容的编写,可以支持导出为`pdf,html`格式,排版内容可预见,避免写完文章后再去查看样式,修改样式,`Markdown`文件都是以`.md`为后缀,可以使用`typora`这款软件来编写,多个平台都有支持.

<!--more-->

#### 常用的一些基本标记格式

**注意通用操作,一般一个标记后要带一个空格才能生效,如果不是会有说明,取消操作通常是使用两次回车即可退出到当前的标记之外.**

##### 段落和换行符

段落,通常是一行或多行连续的文 本,使用回车键可以完成段落分隔,有些编辑器会忽略换行,可以使用`shift + enter`代替

##### 标题

在文本的开头输入 1-6 个`#` 代表六个级别的标题,从 1-6 样式依次减小

##### 引用

在文章中引用其他文章的内容, 在文本开头加入 `>  `

> 我不怕千万人阻挡,只怕自己投降		---  五月天 <<倔强>>

##### 列表

1. 有序列表 : 使用 `1. `即可,回车自动调到第二条,想跳出继续回车即可
2. 无序列表: 使用 `* 或 + `跟有序列表一样的逻辑

##### 任务列表

常用来表示事件是否完成,表现在列表前加一个符号,并且是可以交互的,完成后可以使用鼠标选中或取消,效果如: 

- [ ] 未完成

- [x] 已完成



```
- [ ] //代办事项,注意中括号内空格
- [x] //已办事项
```



##### 代码块

1. 第一种方式高亮显示 , 使用 ` `` `包裹即可 ,使用场景 -- 某些单词或术语
2. 可自定义代码块语言,常用来表示一段代码 使用 ` ``` ` + 语言名 回车即可,结尾使用 ` ``` ` 包裹,一些编辑器会自动识别并包裹,注意要在行首

比如: 

```php
<?php
    echo "php是世界最好的语言!"
```

##### 表格

一般表格由,表头也就是标题,和内容组成,效果如 : 

| 标题1 | 标题2 |
| ----- | ----- |
| 内容1 | 内容2 |

```html
| 标题1 | 标题2 |
| ---- | ---- |
| 内容1 | 内容2 |
//横线和空格缩进根据个人喜好来调整
```

##### 水平线 \\ 分割线

使用连续三个或三个以上的 `----` 或`****` 或 `____`加回车即可 如:

----



##### 链接

1. 指向一个网址: 比如 [github](https://github.com) , 使用方法: 相对路径和绝对路径修改链接地址为相应的地址即可

```html
[链接说明文字](链接地址)
```

2. 链接某一个标题,在小括号中 加上 `# `和标题名即可,如 [标题](#标题)
3. 链接某一个文件的某一个标题,如 [curl常用命令](curl.md#curl常用命令行)

```html
[文件描述](相对或绝对路径#标题名
```

##### 插入图片

图片是建立在链接之上的,在链接的前面加上 `!`即可表示图片,如 

​	![1589680325346](markdown-simple/1589680325346.png)

```
![图片描述](地址)
```

##### 加粗

在文字的两端加上 `** 或 __`可使包裹的文字加粗显示,注意不带空格,效果如 **我粗了**,__我也粗了__

```html
**加粗文字** 
__加粗文字__
```

##### 斜体

在文字的两端加上 `* 或 _`,效果如 : *我斜了*,_我也斜了_

```html
*斜体文字*
_斜体文字_
```

##### 删除线 

在文字的两端加上 `~~` 效果如 : ~~我被删了~~

```html
~~删除文字~~
```



##### 字体样式嵌套

如 ~~**删除线+粗体**~~ ,***斜体加粗体*** , ~~*斜体加删除线*~~

```html
~~**删除线加粗体**~~
***斜体加粗体***
~~*斜体加删除线*~~
//更多组合可以自己尝试一下
```

##### 转义字符

有时只是使用符号的原始表达,并不想要变为样式,使用转义符 `\`

可以被转义的字符

| 字符 |  名称  |
| ---- | :----: |
| \    | 反斜线 |
| `    | 反引号 |
| *    |  星号  |
| _    | 下划线 |
| {}   | 大括号 |
| []   | 中括号 |
| ()   |  括号  |
| #    |  ＃号  |
| +    |  +号   |
| -    |  减号  |
| .    |   点   |
| !    | 感叹号 |
| \|   | 管道符 |









