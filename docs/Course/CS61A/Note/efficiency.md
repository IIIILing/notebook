---
title: 程序效率
tags:
  - CS61A
  - Python
comments: true
statistics: true
---

![](attachments/Pasted%20image%2020250210201022.png)

```python
def fib(n):
  if n==0:
    return 0
  elif n==1:
    return 1
  else:
    return fib(n-1)+fib(n-2)
```

我们想知道要运行多长时间,首先看对fib的调用次数,看看这个次数是怎么增长的

```python
def fib(n):
  if n==0:
    return 0
  elif n==1:
    return 1
  else:
    return fib(n-1)+fib(n-2)

def count(f):
  def counted(n):
    counted.call_count += 1
    return f(n)
  counted.call_count = 0
  return counted

fib = count(fib)
```

```python
def count(f):
  def counted(n):
```

这样的定义是一个高阶函数,它接受一个函数f,返回一个新的函数counted,这个新的函数会调用f,并且记录调用的次数

```python
>>>fib= count(fib)
>>>fib(5)
5
>>>fib.call_count
15
```

## 记忆化

记忆化是加速程序运行时间的一种方法,它是一种保存函数调用结果的方法,当我们再次调用这个函数的时倽,我们可以直接返回这个结果,而不是重新计算一次

```python
def memo(f):
  cache = {}
  #保流了该函数返回值的缓存
  def memoized(n):
    if n not in cache:
      cache[n] = f(n)
      #如果n不在缓存中,就计算f(n)并且保存在缓存中,在n和f(n)的建立对应关系
    return cache[n]
  return memoized
```

这里都用高阶函数,一个接收f,一个接收n

其中f是一个纯函数,如果是一个非纯函数,那么这个缓存就没有意义了(因为非纯函数的结果是不确定的)

```python
def memo(f):
  cache = {}
  #保流了该函数返回值的缓存
  def memoized(n):
    if n not in cache:
      cache[n] = f(n)
      #如果n不在缓存中,就计算f(n)并且保存在缓存中,在n和f(n)的建立对应关系
    return cache[n]
  return memoized

def fib(n):
  if n==0:
    return 0
  elif n==1:
    return 1
  else:
    return fib(n-1)+fib(n-2)

def count(f):
  def counted(n):
    counted.call_count += 1
    return f(n)
  counted.call_count = 0
  return counted
```

输入

```cmd
>>>fib = count(fib)
#这里的fib是一个函数,我们把它传入count函数,返回一个新的函数,这个新的函数会调用fib,并且记录调用的次数
>>>counted_fib = fib
#这里的counted_fib是会记录调用次数的fib函数的别名,它是一个新的函数,它会调用fib,并且记录调用的次数
>>>fib = memo(fib)
#这里的fib是一个函数,我们把它传入memo函数,返回一个新的函数,这个新的函数会调用fib,并且记录已经计算过的结果
>>>fib = count(fib)

>>>fib(30)
832040
#计算了30的斐波那契数列的值
>>>fib.call_count
59
#调用原始函数和调用了记录结果的次数
>>>counted_fib.call_count
31
#调用原始函数的次数
```

为什么一个是31,一个是59呢?

因为我们在调用fib计算的时候,我们第一次会计算出每一个fib(n)的值,包括从fib(n)到fib(0)的值,这个时候我们会调用30次

可以理解成,fib(0)第1次,fib(1)第2次,fib(2)=fib(1)+fib(0)是3次,fib(3)=fib(2)+fib(1)是第4次,以此类推到fib(n-1)调用n次.也就是fib(29)调用了30次

所以fib(30)调用了31次

那59次是怎么来的呢?

把从缓存中取出的次数加上去就是59次

![](attachments/Pasted%20image%2020250210211629.png)'

## 不同方式编写函数的效率

```python
def exp(b,n):
  if n==0:
    return 1
  else:
    return b*exp(b,n-1)
```

这个函数的时间复杂度是O(n),因为每次递归都会调用一次函数,所以时间复杂度是O(n)

```python
def exp(b,n):
  if n==0:
    return 1
  elif n%2==0:
    return exp(b,n/2)**2
  else:
    return b*exp(b,n-1)
```

这一个问题的规模减小很多

  jupyter notebook是一个交互式的编程环境,可以在浏览器中运行代码,并且可以保存代码和运行结果(课上这里用到了jupyter notebook)

## Order of Growth

描述每个函数的增长速度的方式要么用大O表示法,要么用theta表示法

每个增长量级都有不同的表达方式

大O表示法是一种上界,theta表示法是一种上界和下界

n是输入的规模,我们用n来表示输入的规模

最常见的增长量级有:

- O(1)常数时间
- O(logn)对数时间
- O(n)线性时间
- O(nlogn)线性对数时间
- O(n^2)平方时间
- O(2^n)指数时间
- O(n!)阶乘时间
- O(b^n)指数时间

```python
def fib(n):
  if n==0:
    return 0
  elif n==1:
    return 1
  else:
    return fib(n-1)+fib(n-2)
```

这个函数的时间复杂度是O(2^n),因为每次递归都会调用两次函数,所以时间复杂度是O(2^n)

相同增长模式的两个函数在计算相同输入的结果可能花费不完全相同的时间,但是缩放的方式相同

  树递归的时间复杂度是指数时间,因为每次递归都会调用多次函数,所以时间复杂度是指数时间

有正式的系统用于分析函数并且证明他们属于哪种时间复杂度

CS61B中会有更多关于时间复杂度的内容

字典中的元素不影响时间复杂度,因为字典的时间复杂度是O(1)

- 指数时间复杂度的特征是,在n增长的时候,时间复杂度会乘上一个常数

- 二次时间复杂度的特征是,在n增长的时候(规模增大),时间复杂度加上额外的常数倍的n

- 线性时间复杂度的特征是,在n增长的时候,时间复杂度会增加kn倍

- 对数时间复杂度的特征是,在n增长的时候,时间复杂度会增加k倍,k是一个常数和n无关

- 常数时间复杂度的特征是,在n增长的时候,时间复杂度不会增加

如果能实现记忆化某个函数,那么其时间复杂度会降低,从指数时间复杂度降低到线性时间复杂度,或者从线性时间复杂度降低到对数时间复杂度

## Space:内存

在活动环境中的值和函数调用的栈帧都会占用空间,不在活动环境内都会被自动收回释放

而活动环境就是正在运行的函数的环境,也就是我们调用了还没有返回的函数

我们可以写一个函数来看看占用了多少栈帧

```python
def count_frames(f):
  def counted(n):
    counted.open_count += 1
    #当我们调用这个函数的时候,open_count的属性会加1
    counted.max_count = max(counted.open_count, counted.max_count)
    result = f(n)
    #当函数返回之后,open_count的属性会减1,因为函数的栈帧被回收释放
    counted.open_count -= 1
    return result
  counted.open_count = 0
  counted.max_count = 0
  #注意这是函数的属性,我们可以通过函数名.属性名来访问这个属性,也是oop的用法.oop不只局限于class,这也是常见用法
  return counted
```

![](attachments/Pasted%20image%2020250210214723.png)

