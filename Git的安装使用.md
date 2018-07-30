### Git的安装使用

安装和使用参考地址：https://blog.csdn.net/yhl_leo/article/details/50760140

##### 一、Ubuntu环境下安装

1、采用apt方式安装方法

安装参考地址：https://git-scm.com/download/linux

```python
#安装最新稳定版

sudo apt-get install git
（For the latest stable version for your release of Debian/Ubuntu）
#版本安装更新

add-apt-repository ppa:git-core/ppa 
    
sudo apt update; 

sudo apt install git
（For Ubuntu, this PPA provides the latest stable upstream Git version）
```

2、采用源码安装方法

```python
下载源码：

Git源码地址：https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.8.6.tar.xz
    
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.8.6.tar.xz

源码安装：

tar  -xvf  git-2.8.6.tar.xz     &&   cd  git-2.8.6

将git-2.8.6安装到/opt/git-2.8.6目录下，添加用户和组权限

sudo mkdir  /opt/git-2.8.6

sudo chown -Rf  admin  /opt/git-2.8.6     #修改目录对应的用户权限

sudo chgrp  -Rf  admin  /opt/git-2.8.6     #修改目录对应的组权限

(sudo chmod -R 777  文件名  #修改文件权限)

编译安装

./configure  --prefix=/opt/git-2.8.6

make   &&  make install
```

3、Ubuntu环境使用Git

```python
#首先，是指定用户名和邮箱：
$ git config --global user.name "Your Name"
$ git config --global user.email "youremail@domain.com"

#查看Git配置信息：
$ git config --list

#创建一个名为myGitTest的repository仓库区:
$ git init myGitTest

#依次添加文件README和sample.cpp
$ gedit README
$ gedit sample.cpp

#在README文件内随便写入一些内容：
This is my first Git and GitHub test conducted on my Ubuntu Wily system.

#同理，在sample.cpp中写入一段代码：
#include <iostream>
int main()
{
    std::cout << "Hello Git!" << std::endl;
    return 0;
}

#将这两个文件通过git添加到刚刚创建的myGitTest：
$ git add README
$ git add smaple.cpp

#现在，将myGitTest的变化更新情况提交：
$ git commit -m "create a git project"

--------------------------------------------------------------------------

#同步个人项目到GitHub或者码云

#在GitHub个人账户中，创建一个repository

#将新创建的repository的URL地址拷贝

#使用下面的命令，将本地的repository提交到GitHub：

$ git remote add origin https://github.com/yhlleo/myGitTest.git

$ git push origin master

#接着会提示输入GitHub的账户名和密码，输入就可以完成：

#登陆到GitHub上，打开myGitTest如下：
```

##### 二、Windows环境下安装

1 、安装下载参考地址：https://git-scm.com/download/win

```python
#下载官方软件执行程序

#找到安装报后点击安装，安装路径自己选择

#安装完成后生成GitBash、GitCMD、GitGUI三个执行文件
```

2、Windows环境使用Git

```python
#点击GitBash打开操作终端，后使用Linux命令操作

#首先，是指定用户名和邮箱：
$ git config --global user.name "Your Name"
$ git config --global user.email "youremail@domain.com"

#查看Git配置信息：
$ git config --list

#创建一个名为myGitTest的repository仓库区:
$ git init myGitTest

#依次添加文件README和sample.cpp
$ gedit README
$ gedit sample.cpp

#在README文件内随便写入一些内容：
This is my first Git and GitHub test conducted on my Ubuntu Wily system.

#同理，在sample.cpp中写入一段代码：
#include <iostream>
int main()
{
    std::cout << "Hello Git!" << std::endl;
    return 0;
}

#将这两个文件通过git添加到刚刚创建的myGitTest：
$ git add README
$ git add smaple.cpp

#现在，将myGitTest的变化更新情况提交：
$ git commit -m "create a git project"

--------------------------------------------------------------------------

#同步个人项目到GitHub或者码云

#在GitHub个人账户中，创建一个repository

#将新创建的repository的URL地址拷贝

#使用下面的命令，将本地的repository提交到GitHub：

$ git remote add origin https://github.com/yhlleo/myGitTest.git

$ git push origin master

#接着会提示输入GitHub的账户名和密码，输入就可以完成：

#登陆到GitHub上，打开myGitTest如下：
```

