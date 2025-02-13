---
title: 绪论
time: 2024-12-18 14:53:00
description: 一些说明
comments: true
statistics: true
---
## CS61A

这门课是我第二次学了,第一次是在大一的寒假,那个时候还没有学习C语言,感觉学习这门课很吃力,所以后来也是放弃了

但是现在是大二寒假,已经学完了zju的一些CS前置课程,并且自己也了解了C,python等的一些基础使用

在寒假没有动力学习太多专业课的内容,担心学到一半陷入没有正反馈的摆烂境地.所以重新依着兴趣跟着b站的中文翻译重新听一下这门课,希望能比大一的时候走的更远,做完更多的lab和hw

## 笔记结构

按照b站的24fall的视频,这里将笔记分为视频和作业的杂记

作业分为
1. discusssion(12个)
2. homework(10个)
3. lab(12个)
4. project(5个)

目标:2.14前完成学习,并且完成所有的lab和hw

视频来源是b站转载的24fall,所以和在youtube上的分布不太相同,一般按照章节分布笔记

总共37个视频

## ok系统的使用

1. 基本使用
	1. `python ok --score`查看分数
	2. `python ok -q 函数名(不用敲括号)`进行测试

2.  关于debug
	1. Q: How do you prevent the ok autograder from interpreting print statements as output?
	2. Choose the number of the correct choice:
		1) Print with # at the front of the outputted line
		2) You don't need to do anything, ok only looks at returned values, not printed values
		3) Print with 'DEBUG:' at the front of the outputted line

选2,在要输出的栏目之前协商`DEBUG:`


3. 关于交互式命令行
	1. Q: What is the best way to open an interactive terminal to investigate a failing test for question sum_digits in assignment lab01?
	2. Choose the number of the correct choice:
		1) python3 ok -q sum_digits --trace
		2) python3 -i lab01.py
		3) python3 ok -q sum_digits -i
		4) python3 ok -q sum_digits

选2



