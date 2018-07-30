### Python的元类

参考地址：http://python.jobbole.com/88795/

##### 1、元类的理解：

学懂元类，你只需要知道两句话：

- 道生一，一生二，二生三，三生万物
- 我是谁？我从哪来里？我要到哪里去？

道生一，一生二，二生三，三生万物。

1. **道** 即是 type（类型）
2. **一** 即是 metaclass(元类，或者叫类生成器)
3. **二** 即是 class(类，或者叫实例生成器)
4. **三** 即是 instance(实例)
5. **万物** 即是 实例的各种属性与方法，我们平常使用python时，调用的就是它们。

```python
# 创建一个Hello类,拥有属性say_hello,一生二
class Hello(object):
    def say_hello(self,name='world'):
        # 实例属性
        self.name = name
        print('Hello,{}'.format(name))

# 从Hello类创建一个实例hello,即二生三
hello = Hello()

# 使用hello实例调用方法say_hello,即三生万物
hello.say_hello()
print(hello.name)
```

​	这就是一个标准的“二生三，三生万物”过程。 从类到我们可以调用的方法，用了这两步。

```python
# 假如已创建函数
def fun(self,name='world'):
    print('Hello,%s'%(name))

Hello = type('',(object,),dict(say_hello=fun))

# 创建实例,即二生三
hello = Hello()

# 实例调用方法,即三生万物
hello.say_hello()
```

​	这样的写法，就和之前的Class Hello写法作用完全相同，你可以试试创建实例并调用

```python
Hello = type(‘Hello’, (object,), dict(say_hello=fun))
```

- 第一个参数：我是谁。 在这里，我需要一个区分于其它一切的命名，以上的实例将我命名为“Hello”
- 第二个参数：我从哪里来
  在这里，我需要知道从哪里来，也就是我的“父类”，以上实例中我的父类是“object”——python中一种非常初级的类。
- 第三个参数：我要到哪里去
  在这里，我们将需要调用的方法和属性包含到一个字典里，再作为参数传入。以上实例中，我们有一个say_hello方法包装进了字典中。
- 三大永恒命题，是一切类，一切实例，甚至一切实例属性与方法都具有的。理所应当，它们的“创造者”，道和一，即type和元类，也具有这三个参数。
- “道”可以直接生出“二”，但它会先生出“一”，再批量地制造“二”。
- type可以直接生成类（class），但也可以先生成元类（metaclass），再使用元类批量定制类（class）。

```python
# 道生一：传入type
class SayMetaClass(type):
 
    # 传入三大永恒命题：类名称、父类、属性
    def __new__(cls, name, bases, attrs):
        # 创造“天赋”
        attrs['say_'+name] = lambda self,value,saying=name: print(saying+','+value+'!')
        # 传承三大永恒命题：类名称、父类、属性
        return type.__new__(cls, name, bases, attrs)
 
# 一生二：创建类
class Hello(object, metaclass=SayMetaClass):
    pass
 
# 二生三：创建实列
hello = Hello()
 
# 三生万物：调用实例方法
hello.say_Hello('world!')
```

**注意：通过元类创建的类，第一个参数是父类，第二个参数是metaclass**

```python 
# 道生一：传入type
class SayMetaClass(type):
 
    # 传入三大永恒命题：类名称、父类、属性
    def __new__(cls, name, bases, attrs):
        # 创造“天赋”
        attrs['say_'+name] = lambda self,value,saying=name: print(saying+','+value+'!')
        # 传承三大永恒命题：类名称、父类、属性
        return type.__new__(cls, name, bases, attrs)
 
# 一生二：创建类
class Hello(object, metaclass=SayMetaClass):
    pass
 
# 二生三：创建实列
hello = Hello()
 
# 三生万物：调用实例方法
hello.say_Hello('world!')

attrs['say_'+name] = lambda self,value,saying=name:print(saying+','+value+'!')

	它跟据类的名字，创建了一个类方法。比如我们由元类创建的类叫“Hello”，那创建时就自动有了一个叫“say_Hello”的类方法，然后又将类的名字“Hello”作为默认参数saying，传到了方法里面。然后把hello方法调用时的传参作为value传进去，最终打印出来。
```

