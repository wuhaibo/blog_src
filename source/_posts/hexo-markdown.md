---
title: Hexo Markdown
date: 2021-05-27 11:47:30
updated: 2021-05-27 11:47:30
---
title: Hexo Markdown
categories:
  - 技術
tags:
  - markdown
  - blog
date: 2021-05-21 14:23:00
---
> 這篇文章主要介紹markdown的基本語法。 包括文章頭格式, 斜體, 粗體和删除线, 超链接, 圖片, 引用, 无序列表, 有序列表, 表格, 代码块, checkbox等等。
> to be continued...

<!--more-->





## 文章頭格式

``` markdown
---
title: Hexo Markdown
date: 2016-01-15 20:19:32
tags: [Markdown,LaTex,教程]
categories: 学习
toc: true
---
```


## 斜體,粗體和删除线
``` markdown
*斜體* **粗體** ~~刪除的文字~~
```
*斜體* ,
**粗體** ,
~~刪除的文字~~



## 超链接
``` markdown
[链接文字](链接地址 "链接标题")
e.g.: [my blog](http://wuhaibo.github.io)
```
比如: [my blog](http://wuhaibo.github.io)


## 圖片
``` html
<img src="https://images.unsplash.com/photo-1515378791036-0648a3ef77b2?ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8b25saW5lfGVufDB8fDB8fA%3D%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60" width="50%">
```

<img src="https://images.unsplash.com/photo-1515378791036-0648a3ef77b2?ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8b25saW5lfGVufDB8fDB8fA%3D%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60" width="50%">




## 引用


使用 > 表示文字引用：
``` html
> 野火烧不尽,春风吹又生
``` 

> 野火烧不尽,春风吹又生




## 无序列表
``` markdown
+ 无序列表项 一
	- 子无序列表 一
	- 子无序列表 二
		* 子无序列表 三
+ 无序列表项 二
+ 无序列表项 三
``` 

+ 无序列表项 一
	- 子无序列表 一
	- 子无序列表 二
		* 子无序列表 三
+ 无序列表项 二
+ 无序列表项 三




## 有序列表

``` markdown
1. 有序列表项 一
	1. 子有序列表项 一
	2. 子有序列表项 二
2. 有序列表项 二
3. 有序列表项 三
``` 

1. 有序列表项 一
	1. 子有序列表项 一
	2. 子有序列表项 二
2. 有序列表项 二
3. 有序列表项 三


## 表格


绘制表格格式如下,| 控制分列,- 控制分行,: 控制对齐方式。


``` markdown
| Item     | Value     | Qty   |
| :------- | --------: | :---: |
| Computer | 1600 USD  | 5     |
| Phone    | 12 USD    | 12    |
| Pipe     | 1 USD     | 234   |
``` 

| Item     | Value     | Qty   |
| :------- | --------: | :---: |
| Computer | 1600 USD  | 5     |
| Phone    | 12 USD    | 12    |
| Pipe     | 1 USD     | 234   |


## 代码块

``` markdown
用 ``` some codes ```表示的代码块（左侧无空格）

``` 

## checkbox

``` markdown
- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media

``` 

- [x] Write the press release
- [ ] Update the website
- [ ] Contact the media