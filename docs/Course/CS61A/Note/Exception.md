---
title: 错误处理
comments: true
tags:
  - CS61A
  - Python
statistics: true
---
## Raise

`raise<exp>`其中exp我们可以自定义

比如

```python
raise ValueError("This is an error")
```

自动触发的错误类型有：

- `ValueError`
- `TypeError`
- `NameError`

## Try

处理异常的方法

```python
try:
    <try suite>
except <exception type> as <name>:
    <except suite>
```

- `<try suite>`: 代码块，如果没有错误发生，就会执行
- `<exception type>`: 错误类型
- `<name>`: 错误类型的名字
- `<except suite>`: 如果有错误发生，就会执行

```python
try:
    print(1/0)
except ZeroDivisionError as e:
    print("Error:", e)
    x = 1
```

```python
def invert_safe(x):
    try:
        return 1/x
    except ZeroDivisionError as e:
        return str(e)
```

发生错误的时候不会卡住程序，而是会继续执行,但是b被赋值为`str(e)`

## 例子

```python
def reduce(fn, s, initial):
  """
  >>> reduce(mul, [2, 4, 8], 1)
  64
  >>> reduce(add [1, 2, 3, 4], 0)
  10
  """
  for x in s:
    initial = fn(initial, x)
  return initial
```

此时会因为除法除0而报错

```python
def reduce(fn, s, initial):
  """
  >>> reduce(mul, [2, 4, 8], 1)
  64
  >>> reduce(add [1, 2, 3, 4], 0)
  10
  """
  for x in s:
    try:
        initial = fn(initial, x)
    except ZeroDivisionError as e:
        return float('inf')#python中获得无穷大的方式
  return initial
```

或者

```python
def reduce(fn, s, initial):
  if not s:
    return initial
  else:
    first, rest = s[0], s[1:]
    return reduce(f, rest, fn(initial, first))

def divide_all(n, ds):
  try:
    return reduce(truediv, ds, n)
  except ZeroDivisionError as e:
    return float('inf')
```

这样写的好处是,我们在`reduce`中不需要处理错误,而是在`divide_all`中处理错误

我们编写一个函数,这个函数中我们知道正在调用一个可能引发零除错误的函数调用reduce,我们可以在现在写的函数中处理这个错误,而不是在reduce中处理

所以是这个函数知道try reduce之后返回的值,并且处理错误

reduce负责减法,divide_all负责处理错误

