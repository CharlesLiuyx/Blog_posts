---
title: Xpath-Wiki
date: 2017-08-28 12:00:33
categories:
- Tools
tags:
- Wiki
- crawl
---

【阅读时间】查阅类文档
【内容简介】Xpath相关使用法法和例子文档，以供查阅（➜ 后是对应语句的输出output）

<!-- more -->

# XPath 相关例子Note

## 例子1

```python
from lxml import etree
sample1 = """<html>
  <head>
    <title>My page</title>
  </head>
  <body>
    <h2>Welcome to my <a href="#" src="x">page</a></h2>
    <p>This is the first paragraph.</p>
    <!-- this is the end -->
  </body>
</html>
"""
def getxpath(html):
    return etree.HTML(html)
s1 = getxpath(sample1)
```

`//`绝对路径 `text()` 获取内容中的文字信息
```python
s1.xpath('//title/text()') ➜ ['My page']
```

`/` 相对路径
```python
s1.xpath('/html/head/title/text()') ➜ ['My page']
```

获取属性`src`的值
```python
s1.xpath('//h2/a/@src') ➜ ['x']
```

获取所有属性`href`的值
```python
s1.xpath('//@href') ➜ ['#']
```

获取网页中的**所有文本**
```python
s1.xpath('//text()')
➜
['\n  ',
 '\n    ',
 'My page',
 '\n  ',
 '\n  ',
 '\n    ',
 'Welcome to my ',
 'page',
 '\n    ',
 'This is the first paragraph.',
 '\n    ',
 '\n  ',
 '\n']
```

获取网页中的**所有注释**
```python
s1.xpath('//comment()') ➜ [<!-- this is the end -->]
```

## 例子2

```python
sample2 = """
<html>
  <body>
    <ul>
      <li>Quote 1</li>
      <li>Quote 2 with <a href="...">link</a></li>
      <li>Quote 3 with <a href="...">another link</a></li>
      <li><h2>Quote 4 title</h2>Something here.</li>
    </ul>
  </body>
</html>
"""
s2 = getxpath(sample2)
```

获取所有`li`中的文本
```python
s2.xpath('//li/text()') ➜ ['Quote 1', 'Quote 2 with ', 'Quote 3 with ', 'Something here.']
```

获取第一个 第二个`li`中的文本，两种写法均可
```python
s2.xpath('//li[position() = 1]/text()') ➜ ['Quote 1']
```

```python
s2.xpath('//li[1]/text()') ➜ ['Quote 1']
```

```python
s2.xpath('//li[position() = 2]/text()') ➜ ['Quote 2 with ']
```

```python
s2.xpath('//li[2]/text()') ➜ ['Quote 2 with ']
```

奇数 偶数 最后一个
```python
s2.xpath('//li[position() mod2 = 1]/text()') ➜ ['Quote 1', 'Quote 3 with ']
```

```python
s2.xpath('//li[position() mod2 = 0]/text()') ➜ ['Quote 2 with ', 'Something here.']
```

```python
s2.xpath('//li[last()]/text()') ➜ ['Something here.']
```

`li`下面`a`中的文本
```python
s2.xpath('//li[a]/text()') ➜ ['Quote 2 with ', 'Quote 3 with ']
```

`li`下`a`或者`h2`的文本
```python
s2.xpath('//li[a or h2]/text()') ➜ ['Quote 2 with ', 'Quote 3 with ', 'Something here.']
```

使用 | 同时获取 a 和 h2 中的内容
```python
s2.xpath('//a/text()|//h2/text()') ➜ ['link', 'another link', 'Quote 4 title']
```

## 例子3

```python
sample3 = """<html>
  <body>
    <ul>
      <li id="begin"><a href="https://scrapy.org">Scrapy</a>begin</li>
      <li><a href="https://scrapinghub.com">Scrapinghub</a></li>
      <li><a href="https://blog.scrapinghub.com">Scrapinghub Blog</a></li>
      <li id="end"><a href="http://quotes.toscrape.com">Quotes To Scrape</a>end</li>
      <li data-xxxx="end" abc="abc"><a href="http://quotes.toscrape.com">Quotes To Scrape</a>end</li>
    </ul>
  </body>
</html>
"""
s3 = getxpath(sample3)
```

获取 `a` 标签下 `href` 以https开始的
```python
s3.xpath('//a[starts-with(@href, "https")]/text()') ➜ ['Scrapy', 'Scrapinghub', 'Scrapinghub Blog']
```

获取 `href`=https://scrapy.org
```python
s3.xpath('//li/a[@href="https://scrapy.org"]/text()') ➜ ['Scrapy']
```

获取 `id` = begin
```python
s3.xpath('//li[@id="begin"]/text()') ➜ ['begin']
```

获取`text` = Scrapinghub
```python
s3.xpath('//li/a[text()="Scrapinghub"]/text()') ➜ ['Scrapinghub']
```

获取某个标签下 某个参数 = xx
```python
s3.xpath('//li[@data-xxxx="end"]/text()') ➜ ['end']
```

```python
s3.xpath('//li[@abc="abc"]/text()') ➜ ['end']
```

## 例子4

```python
sample4 = u"""
<html>
  <head>
    <title>My page</title>
  </head>
  <body>
    <h2>Welcome to my <a href="#" src="x">page</a></h2>
    <p>This is the first paragraph.</p>
    <p class="test">
    编程语言<a href="#">python</a>
    <img src="#" alt="test"/>javascript
    <a href="#"><strong>C#</strong>JAVA</a>
    </p>
    <p class="content-a">a</p>
    <p class="content-b">b</p>
    <p class="content-c">c</p>
    <p class="content-d">d</p>
    <p class="econtent-e">e</p>
    <!-- this is the end -->
  </body>
</html>
"""
s4 = etree.HTML(sample4)
```

获取 `class` = test 标签中的**所有**文字
```python
s4.xpath('//p[@class="test"]/text()')
➜ ['\n    编程语言', '\n    ', 'javascript\n    ', '\n    ']
```

使用`String`来获得文字段； `strip()` 移除字符串收尾字符，默认为空格
```python
print (s4.xpath('string(//p[@class="test"])').strip())
➜
编程语言python
    javascript
    C#JAVA
```

获取所有`class`属性中**以content开始**的
```python
s4.xpath('//p[starts-with(@class,"content")]/text()') ➜ ['a', 'b', 'c', 'd']
```

获取所有`class`属性中**包含**content的
```python
s4.xpath(('//*[contains(@class,"content")]/text()')) ➜ ['a', 'b', 'c', 'd', 'e']
```





