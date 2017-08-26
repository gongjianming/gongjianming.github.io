---
layout:     post
title:      "BeautifulSoup基础"
subtitle:   ""
date:       2017-04-27 12:12:00
author:     "Alvey"
header-img: "img/home-bg-o.jpg"
header-mask: 0.3
catalog:    true
tags:
    - Python笔记
---

>- Python 2.7 
>- Python中没有自带BS模块，[点击链接](https://pypi.python.org/pypi/b eautifulsoup4/)下载安装
>- Windows压缩包下载完毕后使用cmd命令进行安装，cd到文件夹位置，执行*python setup.py install*

- 导入BeautifulSoup模块
```
from bs4 import BeautifulSoup
```
- 待分析字符串(例子为爱丽丝梦游仙境的一段内容)
```
    html_doc = """html
        <html><head><title>The Dormouse's story</title></head>
    <body>
    <p class="title"><b>The Dormouse's story</b></p>

    <p class="story">Once upon a time there were three little sisters; and their names were
    <a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
    <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
    <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
    and they lived at the bottom of a well.</p>

    <p class="story">...</p>
"""
```
- BeautifulSoup(网站,html解析器,编码方式)
```
soup = BeautifulSoup(html_doc,"html.parser",from_encoding = 'utf-8')
```
- 按照标准html格式输出
```
print(soup.prettify())
```
- 输出第一个title标签
```
print soup.title
```
- 输出第一个标签名
```
print soup.title.name
```
- 输出第一个title标签内容
```
print soup.title.string
```
- 输出第一个title标签父节点标签名
```
print soup.title.parent.name
```
- 输出第一个p标签
```
print soup.p
```
- 输出第一个p标签class属性
```
print soup.p['class']
```
- 输出第一个a标签href属性
```
print soup.a['href']
```
- 修改第一个a标签href属性
```
soup.a['herf'] = 'http://www.baidu.com'
```
- 修改第一个a标签name属性
```
soup.a['name'] = u'百度'
```
- 以列表形式输出所有a标签
```
print soup.find_all('a')
```
- 输出id=link3的a标签
```
print soup.find(id='link3')
```
- 输出所有文字内容（非标签）
```
print soup.get_text()
```
- 获取link的href属性信息
```
for link in soup.find_all('a'):
    print link.get('href') 
```
- 对p的子节点进行循环输出
```
for child in soup.p.children:
    print child
```
- 正则匹配，名字中带有b的标签
```
for tag in soup.find_all(re.compile('b')):
    print tag.name
```
- connents方法获取标签内的内容，不含标签本身
```
print soup.head.contents
```
```
print soup.head.contents[0].contents
```
- 可以对所有tag的子孙节点进行地柜循环
```    
for child in soup.head.descendants:
    print child # descendants
```
