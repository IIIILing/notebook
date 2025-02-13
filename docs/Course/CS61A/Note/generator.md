---
title: 生成器
tags:
  - CS61A
  - Python
comments: true
statistics: true
---


生成器函数是一种特殊类型的迭代器，就像映射对象是一种特殊类型的迭代器一样

特殊之处是从生成器函数中返回的

## 生成器函数

生成器安函数的特点：通过yield关键字返回而不是return返回值

```python

def plus_minus(x)

	yield x
	yield -x
```

```python
t = plus_minus(5)
```

```bash
>>>next(t)
5
>>>next(t)
-5
```

此时，t本身是一个生成器对象，它的工作是帮助迭代调用的函数产生所有值

调用一次，`next(t)`获得的是第一个`yield x`的返回值

第二次调用获得的是第二个`yield -x`获得的z返回值

此时两个yield都被执行了

**生成器函数是一种产生值而不是返回值的函数**

普通函数返回一次，但是生成器函数可以有很多个`yield`，生成器是每次调用生成器函数的时候自动创建的迭代器

这个迭代器会迭代函数的产出

我们可以通过list函数以列表形式打印迭代器

值得注意的是，我们在调用这个生成器函数的时候并没有执行这个函数

而是在调用next的时候才开始执行


执行会在yield处暂停，但是记住函数执行的所有环境，以便于下次调用next的时候，可以继续之前的位置

yield后面如果有语句的话，会在下一个next的时候再被执行，然后执行到下一个yield

## 生成器处理迭代器

yield from 允许生成器从其他迭代器中产生所有元素

也就是yield from可以返回一个迭代器

例如
```python
#我们要写一个函数，a_then_b
#顺序返回迭代器a的值和b的值
"""
>>>list(a_then_b([1,2],[3,4]))
[1,2,3,4]
"""
def a_then_b(a,b):
	for i in a:
		yield i
	for i in b:
		yield i
#这样，再迭代器里顺序&循环返回a和b的值

```

还有这样的写法

```python
def a_then_b
	yield from a
	yield from b
```

这种写法完全等效

可以把yield from 开做事仅仅写了下一个for语句的简化形式

其中遍历了a中的所有元素并且将器yield出来

或许我们需要定义一个从5倒数的生成器函数

```python

def countdown(k):
	if k>0:
		yield k
		yield from countdown(k-1)
```

如果我们不写yield from的形式

那么我们就需要

```python
for i in countdown(k-1)
	yield i
```

可以说明countdown返回的是一个迭代器，yield输出迭代器元素，yield from输出迭代器


下面一个例子更加可以说明yield和yield from的区别了

```python
def prefixes(x):
	yield from prefixes(x[:-1]
	yield x

```

我们随便来一个x = "both"

刚刚进入x函数的时候，就不断进入第一个yield from 里，直到yield from不是迭代器了（也就是把迭代器both运行到b的时候，再x[:-1]已经没有元素，那就返回x也就是b，然后依次递归返回bo，bot，both


用list（prefixes(x))能观察这个迭代器


## 例题

首先我们要如何递归地返回匹配数字问题

```python
"""
>>>partition(4,2)
3
#1+1+1+1=2+1+1=2+2
"""
def count_partitions(n,m)
	if n==0：
		return 1
	elif m == 0:
		return 0
	elif n < 0:
		return 0
	#看似是给初始化的时候解决debug问题，实际是给递归运行到后面返回合适的值
	else:
		exact_match = 0
		if n==m:
			exact_match = 1
		with_n = count_partition(n-m,m)
		with_m = count_partition(n,m-1)
	#要么m-1往下找，要么n-m匹配一次m
		return with_n+with_m+exact_match
```

递归两种可能的情况，然后设定好合适的返回值，用数学归纳法的方式理解递归

现在我们需要list这些结果要怎么写呢

```python

def list_partition(n,m)
	if n<0 or m ==0:
		return []
	else:
		exact_match = []
		if m == n:
			exact_match = [[m]]
		with_m = [p+[m] for p in list_match(n-m,m)]
		#为什么这样写
		#包含m的情况可能成为答案，需要给出列表的格式
		with_n = list_mathc(n,m-1)
		#不包含m，此时不会称为答案，list_match(n,m)-->list_match(n,m-1)->...->list_match(n,1），此时能够成为答案，所以n-m的时候需要写成list的样子)
		return with_n+with_m+exact_match
```

这里我们使用了列表

此时我没有使用yield，使用的是列表，把旧有的列表放到新列表里


如果我们使用yield来实现呢

```python
def partition(n,m)
	if n>0 and m>0
		if n==m:
			#把m变成字符串
			yield str(m）
		for p in partition(n-m,m)
			yield p + " + " str(m)
		yield from partition(n,m-1)
```

yield from 向最终的输出贡献了一整组元素，而yiled贡献了一个值

如果再编写一个程序，有很多种可能新，而我只想要其中一点，有时候生成器函数的方法不仅更加容易编写，并且运行速度也更快




