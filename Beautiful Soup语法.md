### Beautiful Soup语法的使用

参考地址：https://cuiqingcai.com/5548.html

##### 1.简介

​	简单来说，Beautiful Soup就是Python的一个HTML或XML的解析库，可以用它来方便地从网页中提取数据，提高了解析效率，但是其性能较正则要弱。强大的解析工具Beautiful Soup，它借助网页的结构和属性等特性来解析网页。有了它，我们不用再去写一些复杂的正则表达式，只需要简单的几条语句，就可以完成网页中某个元素的提取。

特点：

- Beautiful Soup提供一些简单的、Python式的函数来处理导航、搜索、修改分析树等功能。它是一个工具箱，通过解析文档为用户提供需要抓取的数据，因为简单，所以不需要多少代码就可以写出一个完整的应用程序。
- Beautiful Soup自动将输入文档转换为Unicode编码，输出文档转换为UTF-8编码。你不需要考虑编码方式，除非文档没有指定一个编码方式，这时你仅仅需要说明一下原始编码方式就可以了。
- Beautiful Soup已成为和lxml、html6lib一样出色的Python解释器，为用户灵活地提供不同的解析策略或强劲的速度。

##### 2. 准备工作

​	在开始之前，请确保已经正确安装好了Beautiful Soup和lxml，如果没有安装，可以参考第1章的内容。

##### 3.解析器

​	Beautiful Soup在解析时实际上依赖解析器，它除了支持Python标准库中的HTML解析器外，还支持一些第三方解析器（比如lxml）。lxml解析器有解析HTML和XML的功能，而且速度快，容错能力强，所以推荐使用它。

```python
简单实例：
from bs4 import BeautifulSoup
soup = BeautifulSoup('<p>Hello</p>', 'lxml')
print(soup.p.string)
# 如果使用lxml，那么在初始化Beautiful Soup时，可以把第二个参数改为lxml即可
```

##### 4.基本用法

```python
# 导入类库
from bs4 import BeautifulSoup
# 调用方法
soup = BeautifulSoup('<title>hello,world</title>','lxml')
# 获取title标签
print(soup.title)
# 结果：<title>hello,world</title>
# 获取title标签内容
print(soup.title.string)
# 结果：hello,world
```

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>
"""
# 导入类库
from bs4 import BeautifulSoup
# 调用方法
soup = BeautifulSoup(html,'lxml')
# 使用prettify()方法把html标签html格式化，同时BeautifulSoup方法进行补全html元素
print(soup.prettify())
# 调用soup.title.string方法，先获取title节点，后调用string属性
print(soup.title.string)
```

##### 5.节点选择器

直接调用节点的名称就可以选择节点元素，再调用`string`属性就可以得到节点内的文本了，这种选择方式速度非常快。如果单个节点结构层次非常清晰，可以选用这种方式来解析。

①选择元素

下面再用一个例子详细说明选择元素的方法：当有多个节点时，这种选择方式只会选择到第一个匹配的节点，其他的后面节点都会忽略。

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""
# 导入类库
from bs4 import BeautifulSoup
# 调用方法
soup = BeautifulSoup(html,'lxml')
# 获取第一个title节点标签
print(soup.title)
# 获取类型
print(type(soup.title))
# 获取title标签的内容
print(soup.title.string)
# 无内容返回为空
print(soup.body.string)
# 获取第一个head标签
print(soup.head)
# 获取第一个p标签
print(soup.p)
```

②提取信息

​	上面演示了调用`string`属性来获取文本的值，那么如何获取节点属性的值呢？如何获取节点名呢？下面我们来统一梳理一下信息的提取方式。

(1)获取名称

​	可以利用`name`属性获取节点的名称。这里还是以上面的文本为例，选取`title`节点，然后调用`name`属性就可以得到节点名称：

```python
print(soup.title.name)
结果：title
```

(2)获取属性

​	每个节点可能有多个属性，比如`id`和`class`等，选择这个节点元素后，可以调用`attrs`获取所有属性：

```python
print(soup.p.attrs)
结果：{'class': ['title'], 'name': 'dromouse'}
print(soup.p.attrs['name'])
结果：dromouse
```

​	可以看到，`attrs`的返回结果是字典形式，它把选择的节点的所有属性和属性值组合成一个字典。接下来，如果要获取`name`属性，就相当于从字典中获取某个键值，只需要用中括号加属性名就可以了。比如，要获取`name`属性，就可以通过`attrs['name']`来得到。

​	其实这样有点烦琐，还有一种更简单的获取方式：可以不用写`attrs`，直接在节点元素后面加中括号，传入属性名就可以获取属性值了。样例如下：

```python
print(soup.p['class'])
结果：['title']
print(soup.p['name'])
结果：dromouse
```

​	这里需要注意的是，有的返回结果是字符串，有的返回结果是字符串组成的列表。比如，`name`属性的值是唯一的，返回的结果就是单个字符串。而对于`class`，一个节点元素可能有多个`class`，所以返回的是列表。在实际处理过程中，我们要注意判断类型。

③获取内容

​	可以利用`string`属性获取节点元素包含的文本内容，比如要获取第一个`p`节点的文本：再次注意一下，这里选择到的`p`节点是第一个`p`节点，获取的文本也是第一个`p`节点里面的文本。

```python
print(soup.p.string)
结果：The Dormouse's story
```

④嵌套选择

​	在上面的例子中，我们知道每一个返回结果都是`bs4.element.Tag`类型，它同样可以继续调用节点进行下一步的选择。比如，我们获取了`head`节点元素，我们可以继续调用`head`来选取其内部的`head`节点元素：

```python
html = """
<html><head><title>The Dormouse's story</title></head>
<body>
"""
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.head.title)
print(type(soup.head.title))
print(soup.head.title.string)
```

​	第一行结果是调用`head`之后再次调用`title`而选择的`title`节点元素。然后打印输出了它的类型，可以看到，它仍然是`bs4.element.Tag`类型。也就是说，我们在`Tag`类型的基础上再次选择得到的依然还是`Tag`类型，每次返回的结果都相同，所以这样就可以做嵌套选择了。最后，输出它的`string`属性，也就是节点里的文本内容。

⑤关联选择

​	在做选择的时候，有时候不能做到一步就选到想要的节点元素，需要先选中某一个节点元素，然后以它为基准再选择它的子节点、父节点、兄弟节点等，这里就来介绍如何选择这些节点元素。

```python
print(soup.p.contents)
结果：[]
```

​	可以看到，返回结果是列表形式。`p`节点里既包含文本，又包含节点，最后会将它们以列表形式统一返回。

​	需要注意的是，列表中的每个元素都是`p`节点的直接子节点。比如第一个`a`节点里面包含一层`span`节点，这相当于孙子节点了，但是返回结果并没有单独把`span`节点选出来。所以说，`contents`属性得到的结果是直接子节点的列表。

​	同样，我们可以调用`children`属性得到相应的结果：还是同样的HTML文本，这里调用了`children`属性来选择，返回结果是生成器类型。接下来，我们用`for`循环输出相应的内容。

​	如果要得到所有的子孙节点的话，可以调用`descendants`属性：此时返回结果还是生成器。遍历输出一下可以看到，这次的输出结果就包含了`span`节点。`descendants`会递归查询所有子节点，得到所有的子孙节点。

```python 
#子节点
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.p.children)
for i, child in enumerate(soup.p.children):
    print(i, child)
    
#子孙节点(包含子节点和子孙节点)   
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.p.descendants)
for i, child in enumerate(soup.p.descendants):
    print(i, child)
```

​	如果要获取某个节点元素的父节点，可以调用`parent`属性：这里我们选择的是第一个`a`节点的父节点元素。很明显，它的父节点是`p`节点，输出结果便是`p`节点及其内部的内容。

```python
html = """
<html>
    <body>
        <p class="story">
            <a href="http://example.com/elsie" class="sister" id="link1">
                <span>Elsie</span>
            </a>
        </p>
"""
#父节点
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.a.parent)
#祖先节点
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(type(soup.a.parents))
print(list(enumerate(soup.a.parents)))
```

​	需要注意的是，这里输出的仅仅是`a`节点的直接父节点，而没有再向外寻找父节点的祖先节点。如果想获取所有的祖先节点，可以调用`parents`属性：可以发现，返回结果是生成器类型。这里用列表输出了它的索引和内容，而列表中的元素就是`a`节点的祖先节点。

上面说明了子节点和父节点的获取方式，如果要获取同级的节点（也就是兄弟节点），应该怎么办呢？示例如下：

```python
#兄弟节点
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print('Next Sibling', soup.a.next_sibling)
print('Prev Sibling', soup.a.previous_sibling)
print('Next Siblings', list(enumerate(soup.a.next_siblings)))
print('Prev Siblings', list(enumerate(soup.a.previous_siblings)))
```

可以看到，这里调用了4个属性，其中`next_sibling`和`previous_sibling`分别获取节点的下一个和上一个兄弟元素，`next_siblings`和`previous_siblings`则分别返回所有前面和后面的兄弟节点的生成器。

##### 6. 方法选择器

前面所讲的选择方法都是通过属性来选择的，这种方法非常快，但是如果进行比较复杂的选择的话，它就比较烦琐，不够灵活了。幸好，Beautiful Soup还为我们提供了一些查询方法，比如`find_all()`和`find()`等，调用它们，然后传入相应的参数，就可以灵活查询了。

##### `find_all()`

`find_all`，顾名思义，就是查询所有符合条件的元素。给它传入一些属性或文本，就可以得到符合条件的元素，它的功能十分强大。

它的API如下：

```python
find_all(name , attrs , recursive , text , **kwargs)
```

(1)`name`

我们可以根据节点名来查询元素，示例如下：

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all(name='ul'))
print(type(soup.find_all(name='ul')[0])
```

​	这里我们调用了`find_all()`方法，传入`name`参数，其参数值为`ul`。也就是说，我们想要查询所有`ul`节点，返回结果是列表类型，长度为2，每个元素依然都是`bs4.element.Tag`类型。

​	因为都是`Tag`类型，所以依然可以进行嵌套查询。还是同样的文本，这里查询出所有`ul`节点后，再继续查询其内部的`li`节点：

```python
for ul in soup.find_all(name='ul'):
    print(ul.find_all(name='li'))
```

返回结果是列表类型，列表中的每个元素依然还是`Tag`类型。

接下来，就可以遍历每个`li`，获取它的文本了：

```python
for ul in soup.find_all(name='ul'):
    print(ul.find_all(name='li'))
    for li in ul.find_all(name='li'):
        print(li.string)
```

(2)`attrs`

除了根据节点名查询，我们也可以传入一些属性来查询，示例如下：

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1" name="elements">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all(attrs={'id': 'list-1'}))
print(soup.find_all(attrs={'name': 'elements'}))
```

​	这里查询的时候传入的是`attrs`参数，参数的类型是字典类型。比如，要查询`id`为`list-1`的节点，可以传入`attrs={'id': 'list-1'}`的查询条件，得到的结果是列表形式，包含的内容就是符合`id`为`list-1`的所有节点。在上面的例子中，符合条件的元素个数是1，所以结果是长度为1的列表。

​	对于一些常用的属性，比如`id`和`class`等，我们可以不用`attrs`来传递。比如，要查询`id`为`list-1`的节点，可以直接传入`id`这个参数。还是上面的文本，我们换一种方式来查询：

```python
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all(id='list-1'))
print(soup.find_all(class_='element'))
```

​	这里直接传入`id='list-1'`，就可以查询`id`为`list-1`的节点元素了。而对于`class`来说，由于`class`在Python里是一个关键字，所以后面需要加一个下划线，即`class_='element'`，返回的结果依然还是`Tag`组成的列表。

(3)`text`

`text`参数可用来匹配节点的文本，传入的形式可以是字符串，可以是正则表达式对象，示例如下：

```python
import re
html='''
<div class="panel">
    <div class="panel-body">
        <a>Hello, this is a link</a>
        <a>Hello, this is a link, too</a>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find_all(text=re.compile('link')))
结果：	['Hello, this is a link', 'Hello, this is a link, too']
```

​	这里有两个`a`节点，其内部包含文本信息。这里在`find_all()`方法中传入`text`参数，该参数为正则表达式对象，结果返回所有匹配正则表达式的节点文本组成的列表。

#### `find()`

除了`find_all()`方法，还有`find()`方法，只不过后者返回的是单个元素，也就是第一个匹配的元素，而前者返回的是所有匹配的元素组成的列表。示例如下：

```python
html='''
<div class="panel">
    <div class="panel-heading">
        <h4>Hello</h4>
    </div>
    <div class="panel-body">
        <ul class="list" id="list-1">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
            <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
            <li class="element">Foo</li>
            <li class="element">Bar</li>
        </ul>
    </div>
</div>
'''
from bs4 import BeautifulSoup
soup = BeautifulSoup(html, 'lxml')
print(soup.find(name='ul'))
print(type(soup.find(name='ul')))
print(soup.find(class_='list'))

结果：
<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>
<class 'bs4.element.Tag'>
<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>
```

​	这里的返回结果不再是列表形式，而是第一个匹配的节点元素，类型依然是`Tag`类型。

​	另外，还有许多查询方法，其用法与前面介绍的`find_all()`、`find()`方法完全相同，只不过查询范围不同，这里简单说明一下。

- **find_parents()和find_parent()**：前者返回所有祖先节点，后者返回直接父节点。
- **find_next_siblings()和find_next_sibling()**：前者返回后面所有的兄弟节点，后者返回后面第一个兄弟节点。
- **find_previous_siblings()和find_previous_sibling()**：前者返回前面所有的兄弟节点，后者返回前面第一个兄弟节点。
- **find_all_next()和find_next()**：前者返回节点后所有符合条件的节点，后者返回第一个符合条件的节点。
- **find_all_previous()和find_previous()**：前者返回节点后所有符合条件的节点，后者返回第一个符合条件的节点。