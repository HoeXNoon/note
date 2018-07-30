### Json的使用方法

参考地址：https://mp.weixin.qq.com/s/6LJvCuo7v3HzAay1MME3bg

##### 1、JSON有两种表示结构，对象和数组。

​	JSON的全称是”JavaScript Object Notation”，意思是JavaScript对象表示法，它是一种基于文本，独立于语言的轻量级数据交换格式。

​	对象结构以”{”大括号开始，以”}”大括号结束。中间部分由0或多个以”，”分隔的”key(关键字)/value(值)”对构成，关键字和值之间以”：”分隔，语法结构如代码。

```python
{
    key1:value1,
    key2:value2,
    ...
}
```

其中关键字是字符串，而值可以是字符串，数值，true,false,null,对象或数组

​	数组结构以”[”开始，”]”结束。中间由0或多个以”，”分隔的值列表组成，语法结构如代码。

```python
[
    {
        key1:value1,
        key2:value2 
    },
    {
         key3:value3,
         key4:value4   
    }
]
```

##### 2、json字符串和json对象的区别，比如在js中。

- **字符串：**这个很好解释，指使用“”双引号或’’单引号包括的字符。例如：var comStr = 'this is string';
- **json字符串：**指的是符合json格式要求的js字符串。例如：var jsonStr = "{StudentID:'100',Name:'tmac',Hometown:'usa'}";
- **json对象：**指符合json格式要求的js对象。例如：var jsonObj = { StudentID: "100", Name: "tmac", Hometown: "usa" };

##### 3、使用Python编码和解析Json 

将Python的字典结构导出到json使用`json.dumps()` ，将json读成Python的字典结构，使用`json.loads()` 。

如果不是针对string操作而是对文件操作，分别使用`json.load()`函数和`json.dump()`函数。

```python
import json
# 使用对象格式定义
data1 = {
   'name' : 'ACME',
    'shares' : 100,
    'price' : 542.23,
    'place':'中国'
}
# 生成Json字符串
json_str1 = json.dumps(data1)
print(json_str1)
print(type(json_str1))
# 生成Json文件
json.dump(data1,open('./js1.json',mode='w',encoding='utf-8'),ensure_ascii=False)

# 读取Json字符串
json_dict1 = json.loads(json_str1)
print(json_dict1)
print(type(json_dict1))

# 读取Json文件
json_dict2 = json.load(open('./js1.json',mode='r',encoding='utf-8'))
print(json_dict2)
print(type(json_dict2))


data2 = [
{
   'name' : 'ACME',
    'shares' : 100,
    'price' : 542.23,
    'place':'中国'
},
{
   'name' : 'PEAk',
    'shares' : 100,
    'price' : 542.23,
    'place':'中国'
},
{
   'name' : 'Lenovo',
    'shares' : 100,
    'price' : 542.23,
    'place':'中国'
}
]

# 生成Json字符串
json_str2 = json.dumps(data2)
print(json_str2)
print(type(json_str2))
# 生成Json文件
json.dump(data2,open('./js2.json',mode='w',encoding='utf-8'),ensure_ascii=False)

# 读取Json字符串
json_list1 = json.loads(json_str2)
print(json_list1)
print(type(json_list1))

# 读取Json文件
json_list2 = json.load(open('./js2.json',mode='r',encoding='utf-8'))
print(json_list2)
print(type(json_list2))
```

默认的类型对应如下：

| JSON          | Python    |
| ------------- | --------- |
| object        | dict      |
| array         | list      |
| string        | unicode   |
| number (int)  | int, long |
| number (real) | float     |
| true          | True      |
| false         | False     |
| null          | None      |

##### 4、其他数据类型与Json之间的编码和解码 

​	一般来说，Python对json的解析是list或dict之间的操作，如果需要其他类型与json之间转换，就需要object_hook参数。先定义一个类，将类的字典初始化成json的key-value键值对。这样，json的参数就变成了类的属性。

将一个JSON字典转换为一个Python对象Python

```python
>>> class JSONObject:
...  def __init__(self, d):
...   self.__dict__ = d
...
>>>
>>> data = json.loads(s, object_hook=JSONObject)
>>> data.name
'ACME'
>>> data.shares
50
>>> data.price
490.1
```

还可以通过指定“函数”来进行转换。

用函数来指定序列化的方法，即将对象的“属性-值”对变成字典对，函数返回一个字典，然后`json.dumps`会格式化这个字典。

如果是通过函数将json变成对象，首先获得类名，然后通过`__new__`来创建一个对象（不调用初始化函数），然后将json字典的各个属性赋给对象。

使用函数指定json转换方式Python

```python
def serialize_instance(obj):
 d = { '__classname__' : type(obj).__name__ }
 d.update(vars(obj))
 return d
 
# Dictionary mapping names to known classes
classes = {
 'Point' : Point
}
 
def unserialize_object(d):
 clsname = d.pop('__classname__', None)
 if clsname:
  cls = classes[clsname]
  obj = cls.__new__(cls) # Make instance without calling __init__
  for key, value in d.items():
   setattr(obj, key, value)
  return obj
 else:
  return d
```

使用方法如下：

```python
>>> p = Point(2,3)
>>> s = json.dumps(p, default=serialize_instance)
>>> s
'{"__classname__": "Point", "y": 3, "x": 2}'
>>> a = json.loads(s, object_hook=unserialize_object)
>>> a
<__main__.Point object at 0x1017577d0>
>>> a.x
2
>>> a.y
3
```

**总结（安全性）**

Jsonp(JSON with Padding) 是 json 的一种"使用模式"，可以让网页从别的域名（网站）那获取资料，即跨域读取数据。

为什么我们从不同的域（网站）访问数据需要一个特殊的技术(JSONP )呢？这是因为同源策略。

同源策略，它是由Netscape提出的一个著名的安全策略，现在所有支持JavaScript 的浏览器都会使用这个策略。

- 禁止手动拼接JSON字符串，一律应当用JSON库输出。也不应使用自己实现的ObjectToJson等方法，因为可能有各种没有考虑到的地方。
- jsonp请求的callback要严格过滤，只允许”_”，0到9，a-z, A-Z，即合法的javascript函数的命名。
- jsonp请求也要判断合法性，比如用户是否登陆（这点很容易被忽略）。
- 设置好Content-Type（这点对于调试不方便，但是提高了安全性）。
- 以jsonp方式调用第三方的接口，实际相当于引入了第三方的JS代码，要慎重。