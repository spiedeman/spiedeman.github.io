---
title: 元类（Metaclass）
category: Python
toc: true
mathjax: false
tags:
  - python
abbrlink: 2704906340
date: 2018-01-01 00:00:00
---

# Metaclass
面向对象编程语言中，元类是指实例为类的类。定义确定的类及其实例的行为。并非所有面向对象的语言都支持元类。每一种语言都拥有他自己的**元对象原型**，为一系列描述对象、类、元类之间如何相互作用的规则。

## Python example
在`Python`里，内置的类`type`是一个**元类**。考虑这样一个简单的`Python`类：

```python
class Car(object):
    def __init__(self, make, model, year, color):
        self.make = make
        self.model = model
        self.year = year
        self.color = color

    @property
    def description(self):
        """ Return a description of this car. """
        return "%s %s %s %s" % (self.color, self.year, self.make, self.model)
```

在程序运行时，`Car`本身是类`type`的一个实例，类`Car`的源代码如上所示，有很多内容没有直接呈现出来，例如`Car`对象的大小、在内存中的二进制布局、它们是如何被分配内存的、每当一个`Car`实例被创建时初始化`__init__`方法会被自动调用等等。这些细节不仅在一个新的`Car`实例被创建时，还会在每次访问`car`的属性时起作用。没有元类的语言，这些细节是被语言本身所确定的，且无法被重载。在`Python`中，元类**type**控制了这些细节是实现的方式。具体的控制方式可以用另一个元类代替`type`来改变。

```python
class AttributeInitType(type):
    def __call__(self, *args, **kwargs):
        """ Create a new instance. """

        # First, create the object in the normal default way.
        obj = type.__call__(self, *args)

        # Additionally, set attributes on the new object.
        for name, value in kwargs.items():
            setattr(obj, name, value)

        # Return the new object.
        return obj
```

这个元类只重载了类的创建方式，类的其他方面及对象的行为仍然由`type`进行处理。
现在类`Car`可以使用新定义的元类重写。在`Python2`中通过在类定义中赋值给`__metaclass__`来实现：

```python
class Car(object):
    __metaclass__ = AttributeInitType

    @property
    def description(self):
        """ Return a description of this car. """
        return " ".join(str(getattr(self, attr, "Unknown")) for attr in self.__dict__)
```

在`Python3`中需要提供一个命名参数：

```python
class Car(object, mateclass=AttributeInitType):
    @property
    def description(self):
        """ Return a description of this car. """
        return " ".join(str(value) for value in self.__dict__.values())
```

`Car`的实例于是可以像下面这样实例化：

```python
new_car = Car(make='Toyota', model='Prius', year=2005, color='Green')
old_car = Car(make='Ford', model='Prefect', year=1979)
```

关键字参数实例化的效果也可以不用元类实现：

```python
class Car(object):
    def __init__(self, **kwargs):
        for name, value in kwargs.items():
            setattr(self, name, value)
    @property
    def description(self):
        return " ".join(str(value) for value in self.__dict__.values())
```
