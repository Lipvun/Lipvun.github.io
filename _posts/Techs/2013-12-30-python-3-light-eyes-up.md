---
layout: post
title: Python 3 眼前一亮
category: 技术
tags: [Python,]
keywords: Python 3
description:
---

Python 3 让我眼前一亮 －牛－

### 一、字符编码默认为UTF-8

文件头不需加入coding = utf-8了

### 二、标识符支持非ASCII字符

最牛特性之一，这代表着可以用中文作为变量、函数或类名，咔咔

``` python
class 中国人(object):
    pass

class 客家人(中国人):
    @classmethod
    def 讲(self)
        print('客家话')

客家人.讲()
```

### 三、字符串格式化语法

原有%s %d等不推荐，推荐使用format方法

``` python
d = {'a':'','b':''}
'{a}{b}'.format(d)
```
或静态函数str.format()

其它，bytes对象/有序字典/抽象基类等