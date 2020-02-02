---
title: linuxc
date: 2018-09-03 23:38:09
tags:
---
GDB:
启用调试，在编译时使用-g 参数
运行时参数：1.set args 2.show args
 断点  1.break | b (line or func inter) [if p=5]  2.info b 3.delete | d [range...]
          4.disable | dis | enable | ena [range...]
运行   run | r
单步  next 
函数体内  step （单步） finish(退出内部)
     until 在一个循环体内单步跟踪时，这个命令可以运行程序直到退出循环体,可简写为u。
     continue 继续运行程序，可简写为c
 监视   1.display | undisplay [num]  2.info | disable | enable | delete display [dNo]
disable和enalbe不删除自动显示的设置，而只是让其失效和恢复。
 打印 print | ptype par
显示源代码  1.list |  |  -  |  function | linenum 2. set listsize count | show listsize

GCC COMPLIE:
1.pre compile:cpp -E .i
2.ass file:gcc -S .s
3.bin file:as -c .o
4.link:ld 