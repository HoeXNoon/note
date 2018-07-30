### python的继承、多态、封装

题目：Python 中的继承、多态和封装

涉及问题：

Python 中如何实现多继承，会有什么问题？

Python 中的多态与静态方法有什么区别？

**答案要点如下：**

1.Python 中的继承，就是在定义类时，在括号中声明父类，简单示例如下：

```python
class Father(object): # object 是最基础的一个类，和 JAVA 中的 Object 是一样的
    pass

class Chile(Father): # 继承 Father 类
    pass
```

2.我们都知道，在定义类时，可以通过定义 __init__ 方法来初始化类的属性。有点类似于 JAVA 中的有参构造。但不同的是，在 JAVA 中，子类的构造函数会默认调用父类的无参构造，即在构造函数中的无论你写与不写，第一句代码都是 super() 。而在 Python 中不是的，如果子类不重写 __*init__* 方法，默认会调用父类的 *__init__* 。而一旦子类自己定义了 __init__，则不再会调用父类中的方法，如果想调用，需要手动通过 super() 来调用。示例代码如下：

```python 
class Father(object):
        f_out = "123"
        def __init__(self, f):
                print("father class")
                self.f = f


class Child_1(Father):
        pass

class Child_2(Father):
        # 如果子类中不重写 __init__ 方法，会默认调用父类中的 __init__ 方法
        # 但是如果重写后，则不会自动调用父类中的 __init__ 方法，需要手动来调用
        def __init__(self):
                # 如果子类在初始化参数时，没有父类的参数，则子类不再有父类拥有的实例属性
                # 但类属性仍然会被继承
                print("child class")
                print(self.f_out)
                # 如果想调用父类
                # super(Child_2, self).__init__("123")

# c1 = Child_1() # 如果不传参会报错说接受一个参数，这里就已经说明在调用父类的 __init__
c1 = Child_1("123")  # 输出 father class，且必须传入一个参数
print(c1.f)     # 得到 123

c2 = Child_2() # 不接受参数，因为子类中没有参数
# 输出 child class 123
# print(c2.f) # 报错，找不到 attribute f
```

3.super() 方法的说明，第一个参数是当前类，即子类，第二个参数固定 self，表示当前的实例对象，在 Python2 中必须加参数调用，而 python3 中可以省略参数，如下所示：

```python 
# python2
super(Child, self).父类方法(*args, **kw)

# python3
super().父类方法(*args, **kw)
```

4.Python 相比于 JAVA 更好的一点在于支持多继承，而 JAVA 是单一继承的。在 JAVA 中实现多继承可以通过接口与内部类(这也是我曾经遇到过的一个面试题，有兴趣的可以自行查找资料)。在 Python 实现多继承就很简单了，示例如下：

```python 
class Father():
    pass

class Mother():
    pass

class Child(Father, Mother):
    pass
```

5.需要注意的是，在 Python 中多继承的调用会存在一些问题，有时候会出现我们意想不到的结果，这里不详细展开，感兴趣的可以自己了解，后面会再开文章进行介绍。主要示例代码如下：

```python 
class Base(object):
    def __init__(self):
        print("enter base")
        print('leave base')

class A(Base):
    def __init__(self):
        print('enter A')
        super(A, self).__init__()
        print('leave A')

class B(Base):
    def __init__(self):
        print('enter B')
        super(B, self).__init__()
        print('leave B')

class C(A,B):
    def __init__(self):
        print('enter C')
        super(C, self).__init__()
        print('leave C')

c = C() 
# 输出结果如下
‘’‘
enter C
enter A
enter B
enter base
leave base
leave B
leave A
leave C
’‘’
```

7.Python 中的封装。主要是对属性的封装，采用 __属性名 的形式，(注意是两个下划线)。在 Python 中，以两个下划线开头和结尾，是 Python 中的一些特殊变量，所以我们在私有化属性时，一般不这样定义。而以一个下划线开头的属性，可以通过 实例名. 的方式进行调用，但它有个约定俗成的含义，即：我可以通过 实例. 来调用，但请把我视为 私有变量。(这也很符合 Python 中体现的一切靠自觉的思想)。示例如下：

```python
class Test(object):
        def __init__(self, name, age, sex):
                self.__name = name
                self._age = age
                self.sex = sex

t = Test("Demon", 18, 'M')
# print(t.__name) # 'Test' object has no attribute '__name'
print(t._Test__name) # 可以这样访问到，但不要这样做
print(t._age) # 18
print(t.sex) # M
```

8.Python 中的多态。调用不同对象的相同方法，表现的不一样

在 JAVA 中，用一句话总结多态就是 父类引用指向子类对象。而在 Python 中，父类引用指向子类对象也是多态的一种实现，但不同的是 Python 中多了一种鸭子类型，即如果一个动物，叫起来像鸭子，走起来像鸭子，跑起来像鸭子，那我们就认为它是一只鸭子。即如果一个类，它有和别的类相同的方法，我们就认为它和这个类具有某种关系，是类似的一种类。示例如下：

```python
class Animal(object):
        def run(self):
                print('Animal run')

# 定义一个 Animal 的子类
class Dog(Animal):
        def run(self):
                print('Dog run')

# 定义一个类似 Animal 的类
class Like_Animal(object):
        # 同样具有 run() 方法
        def run(self):
                print('I can run too!!!')

# 定义一个方法，参数是实例对象 ，根据参数来调用对应的 run() 方法
# 也就是我们说的多态
def start_run(run_obj):
        run_obj.run()

# 传入一个 Animal 实例对象
start_run(Animal())

# 传入一个 Dog 实例对象
start_run(Dog())

# 传入一个 类Animal 实例对象
start_run(Like_Animal())
```
9.重载与重写

`重载：函数重载主要解决两个问题：可变参数类型，可变参数个数。即多个同名函数同时存在，具有不同的参数个数/类型`

`重写：参数列表必须完全被重写的方法相同`