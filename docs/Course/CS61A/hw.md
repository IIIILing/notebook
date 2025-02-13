---
title: hw错误点合集
tags:
  - CS61A
  - Python
description: 记录有价值的homework题目和一些零散的知识点
statistics: true
comments: true
---
## hw01

### 指向函数的变量

```python
def a_plus_abs_b(a, b):
    """Return a+abs(b), but without calling abs.
    >>> a_plus_abs_b(2, 3)
    5
    >>> a_plus_abs_b(2, -3)
    5
    >>> a_plus_abs_b(-1, 4)
    3
    >>> a_plus_abs_b(-1, -4)
    3
    """

    if b < 0:
        f = sub
    else:
        f = add
    return f(a, b)
```
这个意思是要返回一个函数指针

f函数赋值了一个函数,返回的时候直接调用并且输入参数ab

### 注意浮点数和整数

```python
def hailstone(n):
    """Print the hailstone sequence starting at n and return its
    length. 
    >>> a = hailstone(10)
    10
    5
    16
    8
    4
    2
    1
    >>> a
    7
    >>> b = hailstone(1)
    1
    >>> b
    1
    """
    "*** YOUR CODE HERE ***"
    count = 0
    while(n != 1):
        print(n)
        if(n%2 == 0):
            n = n//2
        else:
            n = n*3 + 1
        count += 1
    print(n)
    count += 1
    return count
```
在python中没有声明这个数是浮点数还是整数

整数需要`//`才能通过除法得到

浮点数需要`/`就可以得到

### 再次注意python可以给变量赋值成函数

```python
def accumulate(fuse, start, n, term):
    """Return the result of fusing together the first n terms in a sequence
    and start.  The terms to be fused are term(1), term(2), ..., term(n).
    The function fuse is a two-argument commutative & associative function.
    >>> accumulate(add, 0, 5, identity)  # 0 + 1 + 2 + 3 + 4 + 5
    15
    >>> accumulate(add, 11, 5, identity) # 11 + 1 + 2 + 3 + 4 + 5
    26
    >>> accumulate(add, 11, 0, identity) # 11 (fuse is never used)
    11
    >>> accumulate(add, 11, 3, square)   # 11 + 1^2 + 2^2 + 3^2
    25
    >>> accumulate(mul, 2, 3, square)    # 2 * 1^2 * 2^2 * 3^2
    72
    >>> # 2 + (1^2 + 1) + (2^2 + 1) + (3^2 + 1)
    >>> accumulate(lambda x, y: x + y + 1, 2, 3, square)
    19
    """
    "*** YOUR CODE HERE ***"
    result = start
    if(n == 0):
        return result
    if(fuse == add):
        for i in range(1, n+1):
            result = fuse(result, term(i))
        return result
    if(fuse == mul):
        for i in range(1, n+1):
            result = fuse(result, term(i))
        return result
    else:
        for i in range(1, n+1):
            result = fuse(result, term(i))
        return result
```

这里的fuse就代表了传入参数的函数,非常方便

### 函数内构造函数使得可以接受新参数

```python
def make_repeater(f, n):
    """Returns the function that computes the nth application of f.
    >>> add_three = make_repeater(increment, 3)
    >>> add_three(5)
    8
    >>> make_repeater(triple, 5)(1) # 3 * 3 * 3 * 3 * 3 * 1
    243
    >>> make_repeater(square, 2)(5) # square(square(5))
    625
    >>> make_repeater(square, 3)(5) # square(square(square(5)))
    390625
    """
    "*** YOUR CODE HERE ***"
    def repeat(x):
        for _ in range(n):
            x = f(x)
        return x
    return repeat
```

这里为什么不能直接
```python
for _ in range(n)
	f=f(f)
return f
```

因为 `f(f)` 假设 `f` 是一个可以接受自身作为参数的函数，而大多数函数并不满足这个条件

所以我们需要重新声明一个函数,叫`repeat(x)`

此时这个`repeat(x)`函数会得到一个输入值x

这个x是肯定可以当做参数传入`f()`中的(由题意得)

那么我们`x = f(x)`,把函数执行的结果当做参数传入,此时可以得到下一次执行得到的参数

并且我们返回的是`repeat`这个函数,这个函数要接受一个参数

所以我们可以写出`make_repeater(f, n)(x)`的形式的参数

`(f,n)`作为参数传入`make_repeater()`,`(x)`作为参数传入`make_repeater()`函数的内部函数`repeat()`

## hw3:Tree Recursion

### 采用返回值设置递归

>主要是题目不能使用while和赋值符`=`

```python
def num_eights(n):

    """Returns the number of times 8 appears as a digit of n.
    >>> num_eights(3)
    0
    >>> num_eights(8)
    1
    >>> num_eights(88888888)
    8
    >>> num_eights(2638)
    1
    >>> num_eights(86380)
    2
    >>> num_eights(12345)
    0
    >>> num_eights(8782089)
    3
    >>> from construct_check import check
    >>> # ban all assignment statements
    >>> check(HW_SOURCE_FILE, 'num_eights',
    ...       ['Assign', 'AnnAssign', 'AugAssign', 'NamedExpr', 'For', 'While'])
    True
    """
    "*** YOUR CODE HERE ***"
    if n == 0:
        return 0
    else:
        if n % 10 == 8:
            return 1 + num_eights(n//10)
        else:
            return num_eights(n//10)
```
^similar

1. 确定递归返回的值应该怎么选择,当末位是8的时候,返回值+1,其他的时候返回值不变
2. 当递归循环到一定情况下,返回一个真实的值

>下面这个也是例子

```python
def digit_distance(n):

    """Determines the digit distance of n.
    >>> digit_distance(3)
    0
    >>> digit_distance(777)
    0
    >>> digit_distance(314)
    5
    >>> digit_distance(31415926535)
    32
    >>> digit_distance(3464660003)
    16
    >>> from construct_check import check
    >>> # ban all loops
    >>> check(HW_SOURCE_FILE, 'digit_distance',
    ...       ['For', 'While'])
    True
    """
    "*** YOUR CODE HERE ***"
    if n < 10:
        return 0
    else:
        return abs(n%10-(n//10)%10)+digit_distance(n//10)
```

### 找硬币(高阶)
```python
def next_larger_coin(coin):
    if coin == 1:
        return 5
    elif coin == 5:
        return 10
    elif coin == 10:
        return 25

def next_smaller_coin(coin):
    if coin == 25:
        return 10
    elif coin == 10:
        return 5
    elif coin == 5:
        return 1

def count_coins(total):
    """给出总面值,找出用 1, 5, 10, 25大小的硬币能找到几种组合方式
    >>> count_coins(15)
    6
    >>> count_coins(10)
    4
    >>> count_coins(20)
    9
    >>> count_coins(100) # How many ways to make change for a dollar?
    242
    >>> count_coins(200)
    1463
    >>> from construct_check import check
    >>> # ban iteration
    >>> check(HW_SOURCE_FILE, 'count_coins', ['While', 'For'])
    True
    """
    "*** YOUR CODE HERE ***"
    def helper(amount, coin):
        if amount == 0:
            return 1
        elif amount < 0 or coin is None:
            return 0
        else:
            # 使用当前硬币面值，递归计算剩余金额的组合数
            use_coin = helper(amount - coin, coin)
            # 不使用当前硬币面值，递归计算下一个更小面值的组合数
            skip_coin = helper(amount, next_smaller_coin(coin))
            return use_coin + skip_coin
    # 从最大面值 25 开始
    return helper(total, 25)
```

如果没有局限使用递归的话
可以这样写
```python
count = 0  
for m in range(total // 25 + 1):  # 25 分硬币的数量  
    for k in range(total // 10 + 1):  # 10 分硬币的数量  
        for j in range(total // 5 + 1):  # 5 分硬币的数量  
            for i in range(total + 1):  # 1 分硬币的数量  
                if 25 * m + 10 * k + 5 * j + i == total:  
                    count += 1  
return count
```

有递归的话

主要是这两条没想到

```python
	else:
		# 使用当前硬币面值，递归计算剩余金额的组合数
		use_coin = helper(amount - coin, coin)
	    # 不使用当前硬币面值，递归计算下一个更小面值的组合数
		skip_coin = helper(amount, next_smaller_coin(coin))
return use_coin + skip_coin
```

1. 使用当前硬币面值,意思是amount减去自己代表的coin
2. 然后下一步也可以决定是使用当前面值还是使用更小的面值
3. 返回的use_coin和skip_coin都是调用下一轮的helper函数
4. 最后得到的amount如果是负数说明这个排列失败 ,返回0
5. 如果正好是0,那么排列成功,返回1
6. 最后返回值不断相加,得到结果

这样比我们的嵌套排列减少了高时间复杂度

### 限制%判断奇偶,可以用&进行位运算

```python
def interleaved_sum(n, odd_func, even_func):
    """Compute the sum odd_func(1) + even_func(2) + odd_func(3) + ..., up
    to n.
    >>> identity = lambda x: x
    >>> square = lambda x: x * x
    >>> triple = lambda x: x * 3
    >>> interleaved_sum(5, identity, square) # 1   + 2*2 + 3   + 4*4 + 5
    29
    >>> interleaved_sum(5, square, identity) # 1*1 + 2   + 3*3 + 4   + 5*5
    41
    >>> interleaved_sum(4, triple, square)   # 1*3 + 2*2 + 3*3 + 4*4
    32
    >>> interleaved_sum(4, square, triple)   # 1*1 + 2*3 + 3*3 + 4*3
    28
    >>> from construct_check import check
    >>> check(HW_SOURCE_FILE, 'interleaved_sum', ['While', 'For', 'Mod']) # ban loops and %
    True
    """
    "*** YOUR CODE HERE ***"
    def helper(n):
        if n == 1:
            return odd_func(1)
        elif n & 1:  # 判断 k 是否为奇数（通过位运算）
            return odd_func(n) + helper(n - 1)
        else:  # k 为偶数
            return even_func(n) + helper(n - 1)
    return helper(n)
```

## hw4:ADT Tree

### 遍历处理多层列表
```python
def deep_map(f, s):
    """Replace all non-list elements x with f(x) in the nested list s.
    >>> six = [1, 2, [3, [4], 5], 6]
    >>> deep_map(lambda x: x * x, six)
    >>> six
    [1, 4, [9, [16], 25], 36]
    >>> # Check that you're not making new lists
    >>> s = [3, [1, [4, [1]]]]
    >>> s1 = s[1]
    >>> s2 = s1[1]
    >>> s3 = s2[1]
    >>> deep_map(lambda x: x + 1, s)
    >>> s
    [4, [2, [5, [2]]]]
    >>> s1 is s[1]
    True
    >>> s2 is s1[1]
    True
    >>> s3 is s2[1]
    True
    """
    "*** YOUR CODE HERE ***"
```

思路
1. 如果取出的`s[i]`是列表,那么就递归对这个列表使用`deep_map`.
2. 如果取出使用的`s[i]`是数字,那么直接使用`f()`

如何判断列表类型:`type(s[i])==list`

### 树的递归

树是非常适合递归的结构

```python
def max_path_sum(t):

    """Return the maximum root-to-leaf path sum of a tree.

    >>> t = tree(1, [tree(5, [tree(1), tree(3)]), tree(10)])

    >>> max_path_sum(t) # 1, 10

    11

    >>> t2 = tree(5, [tree(4, [tree(1), tree(3)]), tree(2, [tree(10), tree(3)])])

    >>> max_path_sum(t2) # 5, 2, 10

    17

    """

    "*** YOUR CODE HERE ***"

    sum = 0

    if is_leaf(t):

        return label(t)

    else:

        for b in branches(t):

            sum = max(max_path_sum(b), sum)

	    return label(t) + sum
```
比如这个

我们需要找出树根到树叶的的路径中,label的最大和

这样思考
1. 如何处理最后的值:直接返回
2. 如何处理嵌套的树枝:每到一个新树枝,就当做是新的处理对象
3. 如何取最大值,一边计算一边取
### 代码功能
- **输入**：一棵树 `t`，树的节点有一个值（`label`）和若干子节点（`branches`）。
- **输出**：从根节点到叶子节点的路径中，节点值之和最大的那个路径的和。
#### 1. 初始化变量
```python
sum = 0
```
- 这里定义了一个变量 `sum`，用于存储当前子树的最大路径和。初始值为 0。

#### 2. 检查是否为叶子节点
```python
if is_leaf(t):
    return label(t)
```
- `is_leaf(t)` 是一个假设存在的函数，用于检查当前节点 `t` 是否是叶子节点（即没有子节点）。
- 如果是叶子节点，直接返回当前节点的值 `label(t)`，因为叶子节点的路径和就是它自己的值。

#### 3. 递归处理子节点
```python
else:
    for b in branches(t):
        sum = max(max_path_sum(b), sum)
```
- 如果当前节点不是叶子节点，则遍历它的所有子节点 `branches(t)`。
- 对每个子节点 `b`，递归调用 `max_path_sum(b)`，计算以该子节点为根的子树的最大路径和。
- 使用 `max` 函数更新 `sum`，确保 `sum` 始终是当前子树的最大路径和。

#### 4. 返回当前节点的值加上子节点的最大路径和
```python
return label(t) + sum
```
- 最终，函数返回当前节点的值 `label(t)` 加上子节点的最大路径和 `sum`。
- 这是因为从根节点到叶子节点的路径和需要包括当前节点的值。

#### 代码逻辑总结
1. **递归终止条件**：如果当前节点是叶子节点，直接返回它的值。
2. **递归过程**：如果不是叶子节点，递归计算每个子节点的最大路径和，并选择最大的一个。
3. **结果计算**：将当前节点的值加上子节点的最大路径和，作为当前子树的最大路径和
