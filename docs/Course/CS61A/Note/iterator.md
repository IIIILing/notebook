---
title: 迭代器
tags:
  - Python
  - CS61A
statistics: true
comments: true
---
迭代是 Python 最强大的功能之一，是访问集合元素的一种方式

迭代器是一个可以记住遍历的位置的对象,从集合的第一个元素开始访问,直到所有的元素被访问完结束,只能前进不能后退

## 基本方法

### iter() && next()

字符串,列表或者元组对象都可以用于创建迭代器

```
>>> list=[1,2,3,4]
>>> it = iter(list)    # 创建迭代器对象
>>> print (next(it))   # 输出迭代器的下一个元素
1
>>> print (next(it))
2
>>>
```
可以用for语句进行遍历
```python
#!/usr/bin/python3
 
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
for x in it:
    print (x, end=" ")
```
也可以使用next()函数
```python
#!/usr/bin/python3
 
import sys         # 引入 sys 模块
 
list=[1,2,3,4]
it = iter(list)    # 创建迭代器对象
 
while True:
    try:
        print (next(it))
    except StopIteration:
        sys.exit()
```

在迭代的时候如果改变了字典么原来的迭代器不能使用,列表不受影响

```python
>>> d =[1,3,4,5,6]
>>> k = iter(d)
>>> next(k)
1
>>> next(k)
3
>>> d.append(9)
>>> d
[1, 3, 4, 5, 6, 9]
>>> next(k)
4
>>> next(k)
5
>>> next(k)
6
>>> next(k)
9
>>> next(k)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration

```

```python
>>> d={'one':1,'b':2,'c':'wdsidjs1'} 
>>> k = iter(d)
>>> d
{'one': 1, 'b': 2, 'c': 'wdsidjs1'}
>>> k
<dict_keyiterator object at 0x000002734C229080>
>>> next(k)
'one'
>>> next(k)
'b'
>>> d['d']=0
>>> next(k)  
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
RuntimeError: dictionary changed size during iteration
```
### 单次使用

我们使用迭代器只能前进不能后退
```python
>>>r = range(3,6)
>>>ri=iter(r)
>>>for i in ri
...    print(i)
...    
3
4
5
```

另一方面,许多内置的python语句都会返回一个迭代器
![](CS/Python/attachments/Pasted%20image%2020250207091335.png)

## 创建一个迭代器

把一个类所谓一个迭代器使用需要实现两个方法

1. `__iter__()`
2. `__next__()`
`__iter__()` 方法返回一个特殊的迭代器对象， 这个迭代器对象实现了` __next__()` 方法并通过 StopIteration 异常标识迭代的完成


创建一个返回数字的迭代器，初始值为 1，逐步递增 1
```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    x = self.a
    self.a += 1
    return x
 
myclass = MyNumbers()
myiter = iter(myclass)
 
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
print(next(myiter))
```

### StopIteration

`StopIteration`异常用于表示迭代的完成,放置出现无限循环的情况.在next()中我们可以设置在完成指定循环次数之后出发stopiteration异常来结束迭代

```python
class MyNumbers:
  def __iter__(self):
    self.a = 1
    return self
 
  def __next__(self):
    if self.a <= 20:
      x = self.a
      self.a += 1
      return x
    else:
      raise StopIteration
 
myclass = MyNumbers()
myiter = iter(myclass)
 
for x in myiter:
  print(x)
```
20次后结束迭代

```python
while True:
    try:
        print (next(it))
    except StopIteration:
        sys.exit()
```
通过一个无线循环来逐个打印迭代器it里面的元素,直到迭代器耗尽为止

**`try` 块**： 
- `try` 块用于尝试执行可能引发异常的代码。
- 在这里，尝试调用 `next(it)`，即获取迭代器 `it` 的下一个元素
- 如果迭代器中还有元素，`next(it)` 会返回下一个元素。
- 如果迭代器已经耗尽（即没有更多元素），`next(it)` 会抛出 `StopIteration` 异常。
- **`except StopIteration`**：
    - `except` 块用于捕获 `try` 块中可能抛出的异常。
        
    - 如果 `next(it)` 抛出 `StopIteration` 异常，表示迭代器已经耗尽，没有更多元素可以获取。
- **`sys.exit()`**：
    
    - `sys.exit()` 是一个标准库函数，用于终止程序的运行
这样就算出现了报错,也可以得到处理

---
**关于sys.exit()**
- `sys.exit()` 会终止程序的运行，退出代码的执行。 
- 如果你希望在迭代器耗尽后继续执行其他代码，可以将 `sys.exit()` 替换为 `break`，这样会退出循环，但不会终止整个程序

```python
```python
it = iter([1, 2, 3, 4, 5])

while True:
    try:
        print(next(it))
    except StopIteration:
        break  # 退出循环，而不是终止程序
```

---
这段代码假设 `it` 是一个有效的迭代器。如果 `it` 不是迭代器，程序会抛出 `TypeError`

可以通过 `iter()` 函数将可迭代对象（如列表、元组等）转换为迭代器

---

### 生成器:generator

在 Python 中，使用了 **yield** 的函数被称为生成器（generator）
**yield** 是一个关键字，用于定义生成器函数，生成器函数是一种特殊的函数，可以在迭代过程中逐步产生值，而不是一次性返回所有结果

生成器是一个返回迭代器的函数，只能用于迭代操作，更简单点理解生成器就是一个迭代器


```python
def countdown(n):
    while n > 0:
        yield n
        n -= 1
 
# 创建生成器对象
generator = countdown(5)
 
# 通过迭代生成器获取值
print(next(generator))  # 输出: 5
print(next(generator))  # 输出: 4
print(next(generator))  # 输出: 3
 
# 使用 for 循环迭代生成器
for value in generator:
    print(value)  # 输出: 2 1
```

```python
#!/usr/bin/python3
 
import sys
 
def fibonacci(n): # 生成器函数 - 斐波那契
    a, b, counter = 0, 1, 0
    while True:
        if (counter < n): 
            return
        yield a
        a, b = b, a + b
        counter += 1
f = fibonacci(10) # f 是一个迭代器，由生成器返回生成
 
while True:
    try:
        print (next(f), end=" ")
    except StopIteration:
        sys.exit()
```



这里需要补习网课()

```python
def hailstone(n):

    """Q1: Yields the elements of the hailstone sequence starting at n.

       At the end of the sequence, yield 1 infinitely.

  

    >>> hail_gen = hailstone(10)

    >>> [next(hail_gen) for _ in range(10)]

    [10, 5, 16, 8, 4, 2, 1, 1, 1, 1]

    >>> next(hail_gen)

    1

    """

    "*** YOUR CODE HERE ***"

    if n == 1:

        yield 1

        yield from hailstone(1)

    elif n % 2 == 0:

        yield n

        yield  from hailstone(n//2)

    elif n % 2 == 1:

        yield n

        yield from hailstone(3*n+1)
```
使用`yield from`