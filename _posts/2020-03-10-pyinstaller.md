---
layout: post
title: pyinstaller
date: 2020-03-10 15:38:00
categories: code
tags: python exe pyinstaller
excerpt: pyinstaller是一个把python文件转成windows系统下可直接执行的exe文件的库，其原理是把python、引用的库、代码脚本打包到一起。在生成过程中发现，如果程序引用了第三方库，生成的可执行文件可能达几百M，这是由于pyinstaller打包过程中会把多余的库也打包进去。解决这一问题的思路是搭建虚拟环境，安装所需要的库然后打包，这样最终文件会相对纯净...
---

pyinstaller是一个把python文件转成windows系统下可直接执行的exe文件的库，其原理是把python、引用的库、代码脚本打包到一起。

在生成过程中发现，如果程序引用了第三方库，生成的可执行文件可能达几百M，这是由于pyinstaller打包过程中会把多余的库也打包进去。解决这一问题的思路是搭建虚拟环境，安装所需要的库然后打包，这样最终文件会相对纯净，运行速度也会上去。

暂时还没去尝试其他虚拟库是因为如pipenv会被劝退，而Anaconda Navigator可以创建虚拟环境。

创建初试环境后，进入命令行界面完成环境搭建：
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pipinstaller mumpy...
```

cd至目标文件所在目录后进行打包：
```
pyinstaller -F target.py
```

打包完成后，可执行文件会保存在该路径下的Dist/子目录中，在命令行中运行该程序以检验是否有错（否则很可能一闪而过）。例如我报了一个错误：
```
ModuleNotFoundError: No module named 'pkg_resources.py2_warn'
[1688] Failed to execute script pyi_rth_pkgres
```

查了一下是setuptools升级太快版本过高的问题，降低一下版本就行：
```
pip install setuptools==44.0.0
```