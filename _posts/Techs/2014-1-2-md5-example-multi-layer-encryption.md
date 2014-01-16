---
layout: post
title: md5/sha1/base64多层加密示例
category: Techs
tags: [md5, sha1, base64]
keywords: md5, sha1, python
description: md5/sha1/base64多层加密示例
---

MD5和sha1常用于密码/数字签名等加密,为不可逆加密,暂无解密算法.在Python基础类库中包含在hashlib中.示例如下:

``` python
#!/usr/bin/env python
# -*- coding: utf-8 -*-

import hashlib

s = 'Some String'

print s

for i in xrange(1000):
    s = hashlib.md5(s).hexdigest()

print s[:-1]

for i in xrange(1000):
    s = hashlib.sha1(s).hexdigest()

print s[:-1]

hashlib.new('md5',)

import base64

for i in xrange(20):
    s = base64.b64encode(s)
print s

#others
print hashlib.md5(s).hexdigest()
print hashlib.sha1(s).hexdigest()
print hashlib.sha224(s).hexdigest()
print hashlib.sha256(s).hexdigest()
print hashlib.sha384(s).hexdigest()
print hashlib.sha512(s).hexdigest()
```