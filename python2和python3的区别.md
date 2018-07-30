###  python2和python3的区别

##### 1、Python3中print为一个函数，必须用括号括起来；Python2中print为class。

```python 
print '------------python2---------------'
# python2.6以前版本
print 'hello,world' #执行成功
结果：hello,world
print('hello,world') #执行报错
# python2.6以后版本
print 'hello,world' #执行成功
结果：hello,world
print('hello,world') #执行成功
结果：hello,world

print('------------python3---------------')
print 'hello,world' #执行报错
print('hello,world') #执行成功
结果：hello,world
```

##### 2、python2中有 ASCII str() 类型，unicode() 是单独的，不是 byte 类型。python3默认使用utf-8编码字符串。

```python
print '------------python2---------------'
中国 = 'China'  #系统报错
print('------------python3---------------')
# Py3.X源码文件默认使用utf-8编码，这就使得以下代码是合法的： 
中国 = 'China' #执行成功
print(中国)
'''
一、直接在python文件内修改系统编码,默认的编码格式是ascii，我们可以直接修改为utf-8
import sys
reload(sys)
sys.setdefaultencoding('utf-8')
二、在输入输出的时候，修改编码格式
# 解码为GBK，再次编码为UTF-8
html_doc = unicode(html_doc,'GBK').encode('UTF-8')
'''
```

##### 3、通过input()解析用户的输入：（Python3中input得到的为str；Python2的input的到的为int型，Python2的raw_input得到的为str类型）统一一下：Python3中用input，Python2中用row_input，都输入为str。

```python
print '------------python2---------------'
input('只能是int类型数据,否则报错')
raw_input('可以输入数字类型，也可以是字符串类型，都被识别为字符串类型')

print('------------python3---------------')
input('可以输入数字类型，也可以是字符串类型，都被识别为字符串类型')
```

##### 4、整除:(没有太大影响)（Python3中/表示真除，%表示取余，//表示地板除（结果取整）； Python2中/表示根据除数被除数小数点位得到结果，//同样表示地板除）统一一下：Python3中/表示真除，%表示取余，//结果取整；Python2中带上小数点/表示真除，%表示取余，//结果取整。

```python
print '------------python2---------------'
print 1/2 #对于整型，表示取整
print 1.0/2.0 #对于浮点型，表示真除

print('------------python3---------------')
print(1/2)
print(1.0/2.0)
# 二者都表示真除
```

##### 5、在 Python 3 中，range() 是像 xrange() 那样实现以至于一个专门的 xrange() 函数都不再存在（在 Python 3 中xrange() 会抛出命名异常）。在 Python 2 中 xrange() 创建迭代对象的用法是非常流行的。比如： for 循环或者是列表/集合/字典推导式。

```python
原 : range( 0, 4 )   结果是列表 [0,1,2,3 ]
改为：list( range(0,4) )

原 : xrange( 0, 4 )    适用于 for 循环的变量控制
改为：range(0,4)
```

##### 6、try except 异常处理语句的变化

```python
# 原: 
try:
	pass
except Exception, e :
	pass
     	
# 改为：
try:
	pass
except Exception as e:
	pass
```

##### 6、不等运算符

Python 2.x中不等于有两种写法 != 和 <>

Python 3.x中去掉了<>, 只有!=一种写法，还好，我从来没有使用<>的习惯

##### 5.数据类型

1）Py3.X去除了long类型，现在只有一种整型——int，但它的行为就像2.X版本的long

2）新增了bytes类型，对应于2.X版本的八位串，定义一个bytes字面量的方法如下：

```python
>>> b = b'china' 
>>> type(b) 
<type 'bytes'> 
```

str对象和bytes对象可以使用.encode() (str -> bytes) or .decode() (bytes -> str)方法相互转化。

```python
>>> s = b.decode() 
>>> s 
'china' 
>>> b1 = s.encode() 
>>> b1 
b'china' 
```

3）dict的.keys()、.items 和.values()方法返回迭代器，而之前的iterkeys()等函数都被废弃。同时去掉的还有 dict.has_key()，用 in替代它吧 。

##### 7、**打开文件**

原：

```python
file( ..... )
或 
open(.....)
```

改为只能用

```python
open(.....)
```