---
layout: post
title: 由session全局单例模式说起
category: 天光云影
tag: Python
keywords: Python, 单例
description: 由session全局单例模式说起
---

``` python
class Backend(object):
    def __init__(self):
        engine = create_engine("mysql://{0}:{1}@{2}/{3}".format(options.mysql_user, options.mysql_pass, options.mysql_host, options.mysql_db)
                                , pool_size = options.mysql_poolsize
                                , pool_recycle = 3600
                                , echo=options.debug
                                , echo_pool=options.debug)
        self._session = sessionmaker(bind=engine)

    @classmethod
    def instance(cls):
        """Singleton like accessor to instantiate backend object"""
        if not hasattr(cls,"_instance"):
            cls._instance = cls()
        return cls._instance

    def get_session(self):
        return self._session()
```


## 由session全局单例模式说起

把session实例封装在Backend类中，在实际应用中使用方法是，通过类的静态方法instance()创建唯一实例，再调用get_session方法获取唯一session

    Backend.instance().get_session()

单例模式还有其它几种实现方法

### 一、不封装到类中，直接裸露在python模块文件中，最简单也用得最多。

### 二、实现__new__方法

``` python
class Singleton(object):
    """
    Singleton class,将此类的实例绑定在类变量_instance上，若无_instance属性，
    说明此类未实例化，如果已实例化直接返回_instance
    """
    def __new__(cls, *args, **kw):
        if not hasattr(cls, '_instance'):
            cls._instance = super(Singleton,cls).__new__(cls, *args, **kw)
        return cls._instance

class Subclass(Singleton):
    """Subclass,继承单例的__new__方法，如覆写了此方法将无法保持单例^_^"""
    pass

one = Subclass()
two = MyClass()
two.a = 3
print one.a
print one == two
print one is two
```

### 三、共享属性

``` python
class Borg(object):
    """此类所有实例的__dict__属性指向同一字典_state,此方法与二方法都需覆写__new__,功能类似写法不同而已"""
    _state = {}
    def __new__(cls, *args, **kw):
        ob = super(Borg, cls).__new__(cls, *args, **kw)
        ob.__dict__ cls._state
        return ob
```

### 四、使用元类__metaclass__而非继承

``` python
class Singleton2(type):
    """
        方法二的升级版
    """
    def __init__(cls, name, bases, dict):
        super(Singleton2, self).__init__(name, bases, dict)
        self._instance = None
    def __call__(cls, *args, **kw):
        if cls._instance is None:
            cls._instance = super(Singleton2, cls).__call__(*args, **kw)
    return cls._instance

class Myclass(object):
    __metaclass__ = Singleton2
```

五、使用装饰器

``` python

'''些装饰器在PEP318中有'''

def singleton(cls, *args, **kw):
    instance = {}
    def  _singleton():
        if cls not in instance:
            instance[cls] = cls(*args, **kw)
        return instance[cls]
    return _singleton

@singleton
class Myclass(object):
    """用装饰器更pythonic,单例类本身根本不知道自己是单例的，"""
    def __init__(self, x=0):
        self.x = x

m = MyClass()
n = MyClass()
o = n.__class__()
assert m == n
assert m != o
assert n != o
```

#### PS:实际使用时，如遇多线程，参考：

``` python
import threading
class Sing(object):
    def __init__():
        "disable the __init__ method"

    __inst = None # make it so-called private
    __lock = threading.Lock() # used to synchronize code

    @staticmethod
    def getInst():
        Sing.__lock.acquire()
        if not Sing.__inst:
            Sing.__inst = object.__new__(Sing)
            object.__init__(Sing.__inst)
        Sing.__lock.release()
        return Sing.__inst
```