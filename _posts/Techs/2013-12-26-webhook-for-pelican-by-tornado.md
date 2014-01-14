---
layout: post
title: Webhook for pelican by Tornado
category: 技术
tags: [Tornado, Pelican, Webhook]
keywords: [Tornado,Pelican,Webhook]
description: Webhook for pelican by Tornado
---

### Webhook for pelican by Tornado

``` python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
# @Date    : 2013-12-24 16:21:39
# @Author  : Lipvun (me@lipvun.com)
# @Link    : http://lipvun.com
# @Version : 0.1

import os
import subprocess
import tornado.ioloop
import tornado.web

CWDPATH = os.getcwd()

def _call(cmd):
    subprocess.call(cmd.split(),cwd=CWDPATH)

def _update():
    if os.path.isdir(os.path.join(CWDPATH,'_post/.git')):
        _call('git pull')
        if os.path.exists(os.path.join(CWDPATH,'_post/.gitmodules')):
            _call('git submodule init')
            _call('git submodule update')
        return
    if os.path.isdir(os.path.join(CWDPATH,'.hg')):
        _call('hg pull')
    return

class WHHandler(tornado.web.RequestHandler):

    """WebHook Handler"""

    def get(self):
        _update()
        _call('pelican _post -o _deploy -s pelicanconf.py')
        self.write("success.")

    def post(self):
        self.get()

app = tornado.web.Application([
    (r"/webhook",WHHandler),
    ])

if __name__=="__main__":
    app.listen(1888)
    tornado.ioloop.IOLoop.instance().start()
```