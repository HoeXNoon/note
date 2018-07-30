### OS模块的使用

##### 1.OS模块的基本使用方法：

```python 
import os
# 前提是以当前操作文件为参考节点

# 获取当前工作文件路径，不包括当前文件
p1 = os.getcwd()
# print(p1)

# 获取文件夹内文件名的列表目录
p2 = os.listdir('python-os')
# print(p2)

# 判断文件是单独文件还是文件夹
if not os.path.isfile('python-os模块的使用.md'):
	print('hello')
if not os.path.isdir('python-os模块的使用.md'):
	print('world')
if not os.path.isfile('python-os'):
	print('hello')
if not os.path.isdir('python-os'):
	print('world')
    
# 判断文件是否存在，若不存在则建文件
if not os.path.exists('python'):
	os.mkdir('python')
else:
	print('hehe')

# 获取文件绝对路径，全路径
p3 = os.path.abspath(__file__)
print(p3)

# 获取当前工作文件路径，不包括当前文件
p4 = os.path.dirname(__file__)
print(p4)

# 切分最后一个文件，获得元组
p5 = os.path.split('Administrator\Desktop\python-os')
print(p5)

# 对文件后缀进行切割，获得元组
p6 = os.path.splitext('python-os模块的使用.md')
print(p6)

# 对文件进行路径进行连接
p7 = os.path.join(os.path.dirname(__file__),'python-os')
print(p7)
# os.remove('python')
```

##### 2.OS模块的面试题

```python
# (1).补充缺失代码功能
'''
这个函数接收文件夹的名称作为输入参数
返回文件夹中文件的路径以及包含文件中
文件的路径,使用递归方法(多次循环)
'''

def print_directory_content(sPath):
	import os
	# 遍历文件夹中文件目录
	for sChild in os.listdir(sPath):
		# 获得文件的相对路经
		sChildPath = os.path.join(sPath,sChild)
		# 判断文件是目录还是文件
		if os.path.isdir(sChildPath):
			# 递归函数的调用
			print_directory_content(sChildPath)
		else:
			# 打印文件的相对路经
			# print(sChildPath)
			# 打印文件的绝对路经
			print(os.path.abspath(sChildPath))			
print_directory_content('python-os')
```

```python
# 扩展递归方法的使用：
# (1).基本方法
def fun1(n):
	result = 1
	for i in range(1,n+1):
		result = result * i
	return result
ret1 = fun1(3)
# print(ret1)

# (2).高阶函数方法（使用匿名函数）
'''
函数reduce()会对参数序列中元素进行累积。函数将一个数据集合（链表，元组等）中的所有数据进行下列操作：用传给 reduce 中的函数 function（有两个参数）先对集合中的第 1、2 个元素进行操作，得到的结果再与第三个数据用 function 函数运算，最后得到一个结果。

匿名函数是最小函数，只需用表达式而无需申明函数名，lambda语法的定义如下：
lambda [arg1 [,arg2, ... argN]] : expression
'''
from functools import reduce
def fun2(m,n):
	return m*n
# ret2 = reduce(fun2,range(1,4))
ret2 = reduce(lambda x,y:x*y,range(1,4))
# print(ret2)

# (3).递归方法
def fun3(n):
	if n==1:
		return 1
	else:
		return n*fun3(n-1)
ret3 = fun3(5)
# print(ret3)
```

