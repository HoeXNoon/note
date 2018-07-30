# python--__init__()方法和__new__()方法　　

这两个方法是python类中的基本方法，经常会在一些面试中问到。即便没有要面试之类的，学习一下其内部的原理和使用也是有必要的。

 参考地址:https://www.e-learn.cn/content/python/841301

## 首先区分一下这两个方法：

- __init__:初始化方法
- __new__：构造函数

 

 

- __init__:实例方法
- __new__：静态方法

 

- __new__：创建实例，并返回cls实例，也就是init方法的第一参数self
- __init__:在new创建实例对象后调用，self代表创建的这个实例对象，init设置对象属性的初始值，因此是实例方法，并不返回值

 

------

一点小插曲：

　　__new__方法在继承自object的新式类中才有，说到这里，提一下新式类与经典类：

- ​

- - object为所有类的基类
  - 从object继承得来的类称为新式类，没有继承object的为经典类
  - 新式类和经典类的一个常见的差别在于其继承父类的方式，新式类的搜索方式为广度优先，而经典类为深度优先。（当然还有其他的，有兴趣再查一下）
  - python2默认经典类，python3默认新式类，python2可以显示继承object（class A(object)）

------

 

```
1 def __init__(self, *args, **kwargs):
2     pass
3 
4 def __new__(cls, *args, **kwargs):
5     return object.__new__(cls, *args, **kwargs) 
```

 

## 创建实例对象的过程：

　　上面给出了初始化方法和构造函数，可以看到初始化方法没有返回值，而构造函数有返回值。

　　首先，在创建一个类的实例时，先调用构造方法（new），他接受当前正在实例化的类作为第一参数（cls），然后返回创建产生的实例，给__init__方法的第一参数self，让其进行初始值的设置。
　　我们在初学时经常会见到init方法，但是很少见到new方法，这里解释一下，new方法经常不被重写，python中默认调用了父类的new，如果父类没有就继续上溯，直到object的new方法。（既然说是重写了，那么就一定在他的祖先类中有这个方法，最根本的就是object类，他是所有新式类的基类）

　　文字也许看的有点懵，那就举个例子：

```
 1 class A(object):
 2     """docstring for A"""
 3     def __init__(self, *args, **kwargs):
 4         print("init ",self.__class__)
 5 
 6     def __new__(cls, *args,**kwargs):
 7         print("new ",cls)
 8         return object.__new__(cls, *args,**kwargs)
 9 
10 a = A()结果输出：
```

　　new <class '__main__.A'>
　　init <class '__main__.A'>

　　上面的类中定义了init和new，在创建实例对象a时，代码执行只是经过了init的函数定义部分，把这个函数名存在记忆当中，需要用时再调用。然后到了new方法时，接受当前这个类给第一参数cls，来创建对象。创建好对象之后，这个对象的属性方法还什么都没有，现在就返回这个对象（也就是init中的self），来给init来初始化一些内容。如果构造函数并没有返回这个类的对象，那么init方法也就不会被调用。这也符合了上面的输出结果，先输出的new方法的内容，后执行的init方法中的输出。

　　没有返回这个类的对象可以是根本就没有返回值，也可能是返回的对象并不是当前的类的对象，下面会对应举两个例子：

 

![img](https://www.e-learn.cn/sites/default/files/ueditor/1/upload/image/20180629/1530222343545168.png)

　　上面这张截图中，我把上面代码的new方法返回删掉了，现在就只调用的new方法，而没有到init方法内部去。现在这种情况就是new没有返回对象，init方法的第一参数self就没有接收到要初始化的对象，因此并不执行。

 

```
 1 class A(object):
 2     """docstring for A"""
 3     pass
 4 
 5 class B(A):
 6     """docstring for B"Af __init__(self, arg):    """
 7     def __init__(self):
 8         print("init ",self.__class__)
 9 
10     def __new__(cls):
11         print("new ",cls)
12         return object.__new__(A) # 返回的是A的对象，并不是当前类中的实例对象，所以不能调用init方法
```

　　上面这段代码是说构造函数有返回值，但不是当前类的实例对象，因此也不能调用初始化方法。