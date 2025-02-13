---
title: Link list
tags:
  - CS61A
  - Python
comments: true
statistics: true
---

## Link list

每一个链表节点都有一个值和一个指向下一个节点的指针。链表的最后一个节点的指针指向一个特殊的值，如`None`。

在python中,最后一个叫Link empty,指向None.

![](attachments/Pasted%20image%2020250210123049.png)

我们判断链表到头的方法其实是`Link_name==Link.list` 

第一个元素实际上是零元素,是一个attribute value.

```python
Link(1, Link(2, Link(3, Link.empty)))
```

这些结构在python中是通过类来实现的。

```python
class Link:
    empty = ()
    def __init__(self, first, rest=empty):
        assert rest is Link.empty or isinstance(rest, Link)
        self.first = first
        self.rest = rest
    class Link:
    empty = ()
    def __init__(self, first, rest=empty):
        assert rest is Link.empty or isinstance(rest, Link)
        self.first = first
        self.rest = rest
    def __repr__(self):
        if self.rest is not Link.empty:
            rest_repr = ', ' + repr(self.rest)
        else:
            rest_repr = ''
        return 'Link(' + repr(self.first) + rest_repr + ')'

    def __str__(self):
        string = '<'
        while self.rest is not Link.empty:
            string += str(self.first) + ' '
            self = self.rest
        return string + str(self.first) + '>'
```

*`__str__`和`__repr__`*具体是打印输出链表的时候调用的方法.不影响链表的操作.

>关于link.empty是什么,在实现上有选择的余地,可以是None,也可以是().


isinstance() 函数来判断一个对象是否是一个已知的类型，类似 type()。

```python
isinstance(object, classinfo)
```

返回 True 如果 object 是 classinfo 的实例，否则返回 False。

object -- 实例对象。

classinfo -- 可以是直接或间接类名、基本类型或者由它们组成的元组。

```python
>>> Link(3,Link(4,Link(5)))
<__main__.Link object at 0x0000023C93E3DCA0>
>>> s = Link(3,Link(4,Link(5))) 
>>> s.first
3
>>> s.rest
<__main__.Link object at 0x0000023C93E3DF10>
>>> s.rest.first
4
>>> s.rest.rest 
<__main__.Link object at 0x0000023C93E3CC80>
>>> s.rest.rest.first
5
>>> s.rest.rest.rest 
()
>>> s.rest.rest.rest is Link.empty
True
>>>

```

也可以更改链表的值

```python
>>> s.first = 6
>>> s.first
6
>>> s.rest.first = 7
>>> s.rest.first
7
```

```python
>>> s = Link(1, Link(2, Link(3)))
>>> Link(0, s)
<__main__.Link object at 0x0000023C93E3DCA0>
>>> Link(0, s).first
0
>>> Link(0, s).rest
<__main__.Link object at 0x0000023C93E3DF10>
>>> Link(0, s).rest.first
1
```

## 链表的操作

递归在链表中很有用

Range函数,map函数,filter函数都是python内置的,但是不能操作用户定义的Link类.

我们需要自己定义这些函数.

### 例子

首先我们回顾一下我们之前定义过的square函数和odd函数

```python
square = lambda x: x * x
odd = lambda x: x % 2 == 1
```

odd函数返回一个布尔值,表示是否是奇数.

square函数返回一个数的平方.

```python
list(map(square, fliter(odd, range(6))))
```

这个表达式的意思是,对于range(5)中的每一个数,如果是奇数,就返回它的平方,否则返回None.

返回None的情况map函数会自动忽略.

```python
>>> list(map(square, filter(odd, range(6))))
[1, 9, 25]
```

我们可以自己写一个map_link,fliter_link,range_link函数,来操作链表.

```python
map_link(square, fliter_link(odd, range_link(6)))
```

我们一个一个写

```python
def range_link(start, end):
    """Return a Link list of 0, 1, 2, ..., n-1.
    >>> range_link(3,6)
    Link(3, Link(4, Link(5)))
    """
    if start >= end:
        return Link.empty
    return Link(start, range_link(start+1, end))
```

return Link(start, range_link(start+1, end))这一句是递归的关键.

调用了递归,并且做好了结尾的时候返回Link.empty的结束条件.

```python
def map_link(fn, s):
    """Return a Link that contains f(x) for each x in Link s.
    >>> s = range_link(3, 6)
    >>> s
    Link(3, Link(4, Link(5)))
    >>> map_link(lambda x: x*x, s)
    Link(9, Link(16, Link(25)))
    """
    if s is Link.empty:
        return Link.empty
    return Link(fn(s.first), map_link(fn, s.rest))
```

```python
def filter_link(fn, s):
    """Return a Link containing only elements of Link s for which fn returns True.
    >>> s = range_link(3, 6)
    >>> s
    Link(3, Link(4, Link(5)))
    >>> filter_link(lambda x: x % 2 == 1, s)
    Link(3, Link(5))
    """
    if s is Link.empty:
        return Link.empty
        #如果s是空的,直接返回空.
    fliter_rest = filter_link(fn, s.rest)
    #递归调用
    if fn(s.first):
        return Link(s.first, fliter_rest)
        #如果fn通过,把这个值加入Link,并且继续递归
    else:
        return fliter_rest
    #如果fn不通过,这个值跳过,直接返回剩下的部分继续递归
```

## 链表可变性

链表是可变的,我们可以改变链表的值,包括第一个元素和剩下的元素.

```python
>>> s = Link(1, Link(2, Link(3)))
>>> s.first = 6
>>> s.first
6
>>> s.rest.first = 7
>>> s.rest.first
7
>>> s
Link(6, Link(7, Link(3)))
>>> s = s.rest.rest
>>> s.rest.rest.rest.rest.rest.first
7
```

注意,是`s.rest.rest=s`,而不是`s=s.rest.rest`

### 链表排序

```python
def add(s, v):
    """Add a value to an ordered s with no repeats,returning modified s."""
    assert s is Link.empty
    if s.first > v:
        s.first, s.rest = v, Link(s.first, s.rest)
    elif s.first < v and s.rest is Link.empty:
        s.rest = Link(v)
    elif s.first < v:
        s.rest = add(s.rest, v)
    return s
```
