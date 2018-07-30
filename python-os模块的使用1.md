### python中os模块的使用

参考地址：https://www.cnblogs.com/delav/p/7693744.html

​	Python的标准库中的os模块包含普遍的操作系统功能。如果你希望你的程序能够与平台无关的话，这个模块是尤为重要的。即它允许一个程序在编写后不需要任何改动，也不会发生任何问题，就可以在Linux和Windows下运行。

下面列出了一些在os模块中比较有用的部分。它们中的大多数都简单明了。

```python
os.name字符串指示你正在使用的平台。比如对于Windows，它是'nt'，而对于Linux/Unix用户，它是'posix'。
```

```python
os.getcwd() #函数得到当前工作目录，即当前Python脚本工作的目录路径。
```

```python 
os.getenv() #获取一个环境变量，如果没有返回none
```

```python
os.putenv(key, value) #设置一个环境变量值
```

```python
os.listdir(path) #返回指定目录下的所有文件和目录名。
```

```python 
os.remove(path) #函数用来删除一个文件。
```

```
os.system(command) #函数用来运行shell命令。
```

```python 
os.linesep #字符串给出当前平台使用的行终止符。例如，Windows使用'\r\n'，Linux使用'\n'而Mac使用'\r'。
```

```python 
os.curdir: #返回当前目录（'.')
```

```python
os.chdir(dirname): #改变工作目录到dirname
```

os.path常用方法：

```python
os.path.isfile()和os.path.isdir() #函数分别检验给出的路径是一个文件还是目录。
```

```python
os.path.exists() #函数用来检验给出的路径是否真地存在
```

```python
os.path.getsize(name): #获得文件大小，如果name是目录返回0L
```

```python
os.path.abspath(name): #获得绝对路径
```

```python
os.path.normpath(path): #规范path字符串形式
```

```python
os.path.split(path) ：#将path分割成目录和文件名二元组返回。
```

```python
os.path.splitext(): #分离文件名与扩展名
```

```python
os.path.join(path,name): #连接目录与文件名或目录;使用“\”连接
```

```python
os.path.basename(path): #返回文件名
```

```python
os.path.dirname(path): #返回文件路径
```

```python
os.mkdir(path)  
#创建path目录（只能创建一级目录，如'F:\XXX\WWW'）,在XXX目录下创建WWW目录
os.makedirs(path)  
#创建多级目录（如'F:\XXX\SSS'），在F盘下创建XXX目录，继续在XXX目录下创建SSS目录
```
