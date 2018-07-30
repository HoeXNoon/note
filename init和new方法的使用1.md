**1. Python中__new__与__init__方法的区别**

参考地址：https://www.e-learn.cn/content/python/841301

**__new__:**

触发时机： 在实例化对时触发

参数：至少一个cls 接收当前类

返回值：必须返回一个对象实例

作用：实例化对象

方法：静态方法

**注意**：实例化对象是Object类底层实现，其他类继承了Object的__new__才能够实现实例化对象。类级别

**__init__:**

触发时机：初始化对象时触发（不是实例化触发，但是和实例化在一个操作中）

参数：至少有一个self，接收对象

返回值：无

作用：初始化对象的成员

方法：实例方法

**注意**：使用该方式初始化的成员都是直接写入对象当中，类中无法具有。实例级别



```python
class TestCls():
    """docstring for TestCls"""

    def __init__(self, name):
        print('init')
        print(self)
        print(type(self))
        self.name = name

    def __new__(cls, name):
        print('new')
        print(cls)
        print(type(cls))
        return super().__new__(cls)

c = TestCls("CooMark")

先执行new方法创建实例
# new...
# <class '__main__.TestCls'>
# <class 'type'>
返回实例后执行init方法
# init...
# <__main__.TestCls object at 0x02201130>
# <class '__main__.TestCls'>
```

一般来说，”**init**”和”**new**”函数都会有下面的形式：

```python
def __init__(self, *args, **kwargs):
    # func_suite

def __new__(cls, *args, **kwargs):
    # func_suite12345
```

对于”**new**”和”**init**”可以概括为： 
“**new**”方法在Python中是真正的构造方法（创建并返回实例），通过这个方法可以产生一个”cls”对应的实例对象，所以说”**new**”方法一定要有返回。 
对于”**init**”方法，是一个初始化的方法，”self”代表由类产生出来的实例对象，”**init**”将对这个对象进行相应的初始化操作。



**init** 和 **new** 最主要的区别在于：
1.**init** 通常用于初始化一个新实例，控制这个初始化的过程，比如添加一些属性， 做一些额外的操作，发生在类实例被创建完以后。它是实例级别的方法。
2.**new** 通常用于控制生成一个新实例的过程。它是类级别的方法。



