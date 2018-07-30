### python实现单例模式

1.定义：

​	单例模式（Singleton Pattern）**是一种常用的软件设计模式，该模式的主要目的是确保**某一个类只有一个实例存在**。当你希望在整个系统中，某个类只能出现一个实例时，单例对象就能派上用场。所谓单例就是一个类从始至终只能产生一个实例。

应用场景：

​	单例一般都是应用在类似出现连接的情况下。我之前在python web中使用单例的场景是python web服务需要与另一台服务进行连接，需要注册并登陆，这时候如果不使用单例就会出现每一个客户端都会通过web程序向那台服务进行注册和登录，采用了单例之后所有的客户端通过web与另外一台服务只进行一次注册和登录，之后反复使用这个连接，直到该连接失效。

​	比如，某个服务器程序的配置信息存放在一个文件中，客户端通过一个 AppConfig 的类来读取配置文件的信息。如果在程序运行期间，有很多地方都需要使用配置文件的内容，也就是说，很多地方都需要创建 AppConfig 对象的实例，这就导致系统中存在多个 AppConfig 的实例对象，而这样会严重浪费内存资源，尤其是在配置文件内容很多的情况下。事实上，类似 AppConfig 这样的类，我们希望在程序运行期间只存在一个实例对象。项目上线，日志记录也是使用的单例。

根据官方文档：

- __init__是当实例对象创建完成后被调用的，然后设置对象属性的一些初始值。
- __new__是在实例创建之前被调用的，因为它的任务就是创建实例然后返回该实例，是个静态方法。

也就是，__new__在__init__之前被调用，__new__的返回值（实例）将传递给__init__方法的第一个参数，然后__init__给这个实例设置一些参数。

2.实现单例模式的方法。

- 使用模块
- 使用 `__new__`
- 使用装饰器（decorator）
- 使用元类（metaclass）

方法一：使用**__new__**方法

```python 
class SingleTon(object):
	# 在__new__方法中把类实例绑定到类变量_instance上
	def __new__(cls,*args,**kwargs):
		# 如果cls._instance为None表示该类还没有实例化过，实例化该类并返回。
		if not hasattr(cls,'_instance'):
			# 如果cls_instance不为None表示该类已实例化，直接返回cls_instance
			cls._instance = object.__new__(cls,*args,**kwargs)
		return cls._instance

class TestClass(SingleTon):
	a = 1

test1 = TestClass()
test2 = TestClass()
# 内存地址相同
print(id(test1))
print(id(test2))
print(test1.a,test2.a)
test1.a = 2
# 返回值相同
print(test1.a,test2.a)


class Singleton(object):
    _instance = None
    def __new__(cls, *args, **kw):
        if not cls._instance:
            cls._instance = super(Singleton, cls).__new__(cls, *args, **kw)  
        return cls._instance  
 
class MyClass(Singleton):  
    a = 1
```

​	在上面的代码中，我们将类的实例和一个类变量 `_instance` 关联起来，如果 `cls._instance`为 None 则创建实例，否则直接返回 `cls._instance`。

方法二：使用装饰器(decorator)

```python 
def SingleTon(cls,*args,**kwargs):
	instances = {}
	def _singleton():
		if cls not in instances:
			instances[cls] = cls(*args,**kwargs)
		return instances[cls]
	return _singleton

@SingleTon
class TestClass(object):
	a = 1

test1 = TestClass()
test2 = TestClass()
print(id(test1),id(test2))
print(test1.a,test2.a)
test1.a = 2
print(test1.a,test2.a)


from functools import wraps 
def singleton(cls):
    instances = {}
    @wraps(cls)
    def getinstance(*args, **kw):
        if cls not in instances:
            instances[cls] = cls(*args, **kw)
        return instances[cls]
    return getinstance
 
@singleton
class MyClass(object):
    a = 1
```

​	我们知道，装饰器（decorator）可以动态地修改一个类或函数的功能。这里，我们也可以使用装饰器来装饰某个类，使其只能生成一个实例。

​	在上面，我们定义了一个装饰器 `singleton`，它返回了一个内部函数 `getinstance`，该函数会判断某个类是否在字典 `instances` 中，如果不存在，则会将 `cls` 作为 key，`cls(*args, **kw)` 作为 value 存到 `instances` 中，否则，直接返回 `instances[cls]`。

方法三：使用**__metaclass__**（元类）

元类（metaclass）可以控制类的创建过程，它主要做三件事：

- 拦截类的创建
- 修改类的定义
- 返回修改后的类

使用元类实现单例模式的代码如下：

```python
class Singleton(type):
    _instances = {}
    def __call__(cls, *args, **kwargs):
        if cls not in cls._instances:
            cls._instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
        return cls._instances[cls]
 
# Python2
class MyClass(object):
    __metaclass__ = Singleton
 
# Python3
# class MyClass(metaclass=Singleton):
#    pass
```

方法四：使用模块

​	其实，**Python 的模块就是天然的单例模式**，因为模块在第一次导入时，会生成 `.pyc` 文件，当第二次导入时，就会直接加载 `.pyc` 文件，而不会再次执行模块代码。因此，我们只需把相关的函数和数据定义在一个模块中，就可以获得一个单例对象了。如果我们真的想要一个单例类，可以考虑这样做：

```python
# mysingleton.py
class My_Singleton(object):
    def foo(self):
        pass 
my_singleton = My_Singleton()

from mysingleton import my_singleton
 
my_singleton.foo()
```

