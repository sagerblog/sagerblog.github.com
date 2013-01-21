---
layout: post
title: "Python循环遍历所有文件并改名"
date: 2013-01-21 18:34
comments: true
categories: [python, file]
---
我希望将某个目录下的所有jpeg文件改名为jpg结尾的文件，可以利用os.walk功能以及os.rename两个功能

```python walk all files in special directory
#!/usr/bin/python
import os

def walkDir(rootdir=None):
    print rootdir
    count = 0
    for parent, dirnames, filenames in os.walk(rootdir):
        #for dirname in dirnames:
        #    print "parent is:" + parent
        #    print "dirname is:" + dirname
        for filename in filenames:
            if filename[-5:] == ".jpeg":
            src_file = os.path.join(parent, filename)
            dest_file = src_file[:-5] + ".jpg"
            os.rename(src_file, dest_file)
        count += 1
        print 'Rename total file count is:', count

def main():
    walkDir(os.getcwd())

if __name__ == "__main__":
    main()

```
<!--more-->
利用walk还可以做很多其他的事情，例如对所有python源代码进行编译，以便发布打包的时候使用遍以后的二进制文件，而不是源代码。
可以使用如下代码：
```python Compile python source code
py_compile.compile(filename)
```
