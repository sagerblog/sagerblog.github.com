---
layout: post
title: "python计算文件行数的三种方法"
date: 2013-01-21 18:26
comments: true
categories: [python, os, file]
---
##Python计算文件行数的三种方法##
```python Caculate file lines
import timeit,os
def linecount_1():
    return len(open('data.sql').readlines())    #最直接的方法

def linecount_2():
    count = -1 #让空文件的行号显示0
    for count,line in enumerate(open('data.sql')): pass
    #enumerate格式化成了元组,count就是行号,因为从0开始要+1
    return count+1
 
def linecount_3():
    count = 0
    thefile = open('data.sql','rb')
    while 1:
        buffer = thefile.read(65536)
        if not buffer:break
        count += buffer.count('\n') #通过读取换行符计算
    return count
 
for f in linecount_1,linecount_2,linecount_3:
    print f.__name__,f()
     
    for f in linecount_1,linecount_2,linecount_3:
        #第一个参数是函数,第二个参数是环境，例如import模块
        t = timeit.Timer(f,'')      
        #第一个参数是重复整个测试的次数，第二个参数是每个测试中调用被计时语句的次数。
        print min(t.repeat(100,3))  
```
<!--more-->
###结果：###
linecount_1 43119

linecount_2 43119

linecount_3 43119

0.04646344717

0.0403758019525

0.0126613857348

