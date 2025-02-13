---
title: 实验题
tags:
  - CS61A
  - Python
description: 记录有价值的lab题目和一些零散的知识点
comments: true
statistics: true
---
lab00的内容过于基础,关于ok系统的内容可以在绪论里找到

## lab01

- 注意`for i in range(a,b,c)`时,步长为c,开头为a(包含),结尾为b(不含)

判断是否存在两个连在一起的数
```python
def double_eights(n):
    """Return true if n has two eights in a row.
    >>> double_eights(8)
    False
    >>> double_eights(88)
    True
    >>> double_eights(2882)
    True
    >>> double_eights(880088)
    True
    >>> double_eights(12345)
    False
    >>> double_eights(80808080)
    False
    """
    "*** YOUR CODE HERE ***"
    count = 0
    while n > 0:
        if n % 10 == 8:
            count += 1
        else:
            count = 0
        n = n // 10
        if count == 2:
            return True
    if count < 2:
        return False
```

### lab1-optional

进入交互式解释器时,会打印输出函数的返回值

并且如果是字符串形式的话,会原样返回`''`或者`""`

## lab2

### Test

在Python中，`or` 运算符的行为是返回第一个为真的操作数，如果所有操作数都为假，则返回最后一个操作数。

```python
>>> -1 or 6
-1
```

高阶函数的多重调用,搞清楚每一层次的括号是什么含义的参数

```python
def cycle(f1, f2, f3):
    """Returns a function that is itself a higher-order function.
    >>> def add1(x):
    ...     return x + 1
    >>> def times2(x):
    ...     return x * 2
    >>> def add3(x):
    ...     return x + 3
    >>> my_cycle = cycle(add1, times2, add3)
    >>> identity = my_cycle(0)
    >>> identity(5)
    5
    >>> add_one_then_double = my_cycle(2)
    >>> add_one_then_double(1)
    4
    >>> do_all_functions = my_cycle(3)
    >>> do_all_functions(2)
    9
    >>> do_more_than_a_cycle = my_cycle(4)
    >>> do_more_than_a_cycle(2)
    10
    >>> do_two_cycles = my_cycle(6)
    >>> do_two_cycles(1)
    19
    """
    "*** YOUR CODE HERE ***"
    def higher_order_function(n):
        def inner_function(x):
            if n >= 1:
                x = f1(x)
            if n >= 2: 
                x = f2(x)
            if n >=3
                x = f3(x)
            return x
        return inner_function
    return higher_order_function
```

## lab03

注意:
1. 当我们写`for n in s`时,n自己就是列表的元素,不需要`s[n]`
2. 并且如何判断数字为平方数,`int(sqrt(n))==sqrt(n)`
3. 注意`return 1`,和`return True`不一样

```python
def double_eights(n):

    """ Returns whether or not n has two digits in row that

    are the number 8. Assume n has at least two digits in it.

  

    >>> double_eights(1288)

    True

    >>> double_eights(880)

    True

    >>> double_eights(538835)

    True

    >>> double_eights(284682)

    False

    >>> double_eights(588138)

    True

    >>> double_eights(78)

    False

    >>> from construct_check import check

    >>> # ban iteration

    >>> check(LAB_SOURCE_FILE, 'double_eights', ['While', 'For'])

    True

    """

    "*** YOUR CODE HERE ***"

    if n == 0:

        return False

    else:

        if n % 10 == 8:

            if (n // 10) %10 == 8:

                return True

            elif n // 10 == 0:

                return False

            else:

                return double_eights(n // 10)

        else:

            return double_eights(n // 10)
```

[hw](Course/CS61A/hw.md#^similar)

## lab04

### 字典

1. 使用`.key()`或者`.values()`或者`.item()`访问得到键或值或键值对的序列
2. 可以使用`in`来检查字典里是否含有键
3. 字典推导值是结算结果为新字典的表达式

**T1:字典推导式**
```python
def divide(quotients, divisors):

    """Return a dictonary in which each quotient q is a key for the list of

    divisors that it divides evenly.

  

    >>> divide([3, 4, 5], [8, 9, 10, 11, 12])

    {3: [9, 12], 4: [8, 12], 5: [10]}

    >>> divide(range(1, 5), range(20, 25))

    {1: [20, 21, 22, 23, 24], 2: [20, 22, 24], 3: [21, 24], 4: [20, 24]}

    """

    return {x: [y for y in divisors if y % x == 0] for x in quotients}
```

字典推导式如果值是字典的话,一般需要继续写一个字典,来推导


### 递归难题:找到水果的匹配方式

**计算并打印购买不同水果的方案**，使得所花金额等于 `total_amount`，并且**所有必需的水果（`required_fruits`）至少购买一次**。

---

#### **1. 代码结构概览**

主要包含两个函数：

1. **`display(fruit, count)`**
    - 负责**格式化**水果的数量显示（单数和复数处理）。
2. **`buy(required_fruits, prices, total_amount)`**
    - 通过**递归回溯**计算符合条件的购买方案。

---

#### **2. `display(fruit, count)` 解释**

```python
def display(fruit, count):
    """Display a count of a fruit in square brackets.

    >>> display('apples', 3)
    '[3 apples]'
    >>> display('apples', 1)
    '[1 apple]'
    >>> display('banana', 2)
    '[2 bananas]'
    >>> display('banana', 1)
    '[1 banana]'
    """
    assert count >= 1, "Count must be at least 1"
    
    if count == 1 and fruit.endswith('s'):
        fruit = fruit[:-1]  # Convert plural to singular
    elif count > 1 and not fruit.endswith('s'):
        fruit += 's'  # Convert singular to plural
    
    return f'[{count} {fruit}]'
```

**作用**：

- 接收 `fruit` 和 `count`，返回格式化后的 `[数量 水果]` 字符串。
- 处理单数和复数：
    - 若 `count == 1` 且 `fruit` 以 "s" 结尾（如 `"apples"`），去掉 "s"（变成 `"apple"`）。
    - 若 `count > 1` 且 `fruit` 不是复数，加上 "s"（如 `"banana"` → `"bananas"`）。
- 例如：
    
    ```python
    display('apples', 1)   # '[1 apple]'
    display('apples', 3)   # '[3 apples]'
    display('banana', 1)   # '[1 banana]'
    display('banana', 2)   # '[2 bananas]'
    ```
    

---

#### **3. `buy(required_fruits, prices, total_amount)` 解释**

```python
def buy(required_fruits, prices, total_amount):
    """Print ways to buy some of each fruit so that the sum of prices is amount."""
```

- `required_fruits`：必须购买的水果列表（如 `['apples', 'oranges', 'bananas']`）。
- `prices`：字典，存储水果的单价（如 `{'oranges': 4, 'apples': 3, 'bananas': 2}`）。
- `total_amount`：总金额（目标值）。

##### **递归函数 `add`**

```python
    def add(fruits, amount, cart, selected):
        if not fruits and amount == 0 and set(selected) == set(required_fruits):
            print(cart)
```

- **终止条件**：
    - `not fruits`：所有水果都被遍历。
    - `amount == 0`：已花费的金额等于 `total_amount`。
    - `set(selected) == set(required_fruits)`：确保所有 `required_fruits` 至少购买一次。
    - **若满足以上条件，打印当前组合（`cart`）**。

---

##### **递归逻辑**

```python
        elif fruits and amount >= 0:
            fruit = fruits[0]
            price = prices[fruit]
            for k in range(amount // price + 1):
                new_cart = cart + display(fruit, k) if k > 0 else cart
                new_selected = selected | {fruit} if k > 0 else selected
                add(fruits[1:], amount - k * price, new_cart, new_selected)
```

- **`fruit = fruits[0]`**：取当前水果。
    
- **`price = prices[fruit]`**：获取水果单价。
    
- **循环 `k` 取值范围**：
    
    ```python
    for k in range(amount // price + 1):
    ```
    
    - `k` 代表**买多少个 `fruit`**。
    - `range(amount // price + 1)` 计算**最大可买数量**。
    - 例如 `price['apples'] = 3`，`total_amount = 12`，最多能买 `12//3=4` 个 `apples`。
- **递归调用 `add` 继续处理下一个水果**：
    
    ```python
    add(fruits[1:], amount - k * price, new_cart, new_selected)
    ```
    
    - **减少 `amount`**：`amount - k * price`（扣除已购买水果的花费）。
    - **更新 `cart`**：
        
        ```python
        new_cart = cart + display(fruit, k) if k > 0 else cart
        ```
        
        - 只有 `k > 0` 时才更新 `cart`（即**至少买了 1 个水果**）。
    - **更新 `selected`**：
        
        ```python
        new_selected = selected | {fruit} if k > 0 else selected
        ```
        
        - `selected` 追踪已购买的水果。
        - `|` 是集合并运算符，`selected | {fruit}` 确保 `fruit` 被加入。

---

#### **4. 运行示例**

```python
prices = {'oranges': 4, 'apples': 3, 'bananas': 2, 'kiwis': 9}
buy(['apples', 'oranges', 'bananas'], prices, 12)
```

##### **递归过程**

|`fruit`|`amount`|`k` 取值|递归路径|
|---|---|---|---|
|apples|12|0~4|继续递归|
|oranges|12-3k|0~3|继续递归|
|bananas|12-3k-4j|0~n|终止并打印|

最终输出：

```
[2 apples][1 orange][1 banana]
```

---

#### **5. 关键点总结**

1. **回溯搜索**：递归遍历每种可能的购买方式。
2. **确保所有水果都至少购买一次**：
    - 通过 `set(selected) == set(required_fruits)` 约束解。
3. **动态构造 `cart`**：
    - `display(fruit, k)` 格式化购买方案，确保输出 `[数量 水果]`。

---

## **6. 代码优化点**

- 目前 `new_cart` 拼接字符串的方式**可能会导致额外的空字符串**，可以调整为：
    
    ```python
    new_cart = cart + (display(fruit, k) if k > 0 else '')
    ```
    
- 这能确保 `cart` 在 `k == 0` 时不被错误拼接。

---

## lab05

### 列表的可变操作

列表和字段是可变的,最常见的两个操作是item assignment 和append

item assignment指的是吧一个列表赋值给另外一个变量

还有许多其他列表更改方法

1. append(elem):将elem添加到列表末尾返回none
2. extend(s):将 iterable `s` 的所有元素添加到列表的末尾,返回 `None`
3. `insert(i, elem)`：在索引 `i` 处插入 `elem`。如果 `i` 大于或等于列表的长度，则 `elem` 插入到末尾。这不会替换任何现有元素，而只会添加新元素 `elem`.返回 `None`
4. pop(i)删除并且返回索引处为i的元素
5. pop()删除并且返回最后一个元素
6. `remove(elem)`：删除列表中第一次出现的 `elem`。返回 `None`。如果 `elem` 不在列表中，则出错
7. `insert(i, elem)`：在索引 `i` 处插入 `elem`。如果 `i` 大于或等于列表的长度，则 `elem` 插入到末尾。这不会替换任何现有元素，而只会添加新元素 `elem`。返回 `None`

注意
```python
>>>s = [4,5]
>>>print(s.append(5))
None
```
因为成功添加返回None,不成功添加非0返回


## lab05

```python
def insert_items(s, before, after):

    """Insert after into s after each occurrence of before and then return s.

  

    >>> test_s = [1, 5, 8, 5, 2, 3]

    >>> new_s = insert_items(test_s, 5, 7)

    >>> new_s

    [1, 5, 7, 8, 5, 7, 2, 3]

    >>> test_s

    [1, 5, 7, 8, 5, 7, 2, 3]

    >>> new_s is test_s

    True

    >>> double_s = [1, 2, 1, 2, 3, 3]

    >>> double_s = insert_items(double_s, 3, 4)

    >>> double_s

    [1, 2, 1, 2, 3, 4, 3, 4]

    >>> large_s = [1, 4, 8]

    >>> large_s2 = insert_items(large_s, 4, 4)

    >>> large_s2

    [1, 4, 4, 8]

    >>> large_s3 = insert_items(large_s2, 4, 6)

    >>> large_s3

    [1, 4, 6, 4, 6, 8]

    >>> large_s3 is large_s

    True

    """

    "*** YOUR CODE HERE ***"

    indices = [i for i, x in enumerate(s) if x == before]

    for i in reversed(indices):

        s.insert(i + 1, after)

    return s
```
为什么要用`reversed`函数来翻转列表,确保每次插入都在正确的位置进行,而不会干扰到其他部分的逻辑.

注意append的用法,在python中

我们先声明`grouped = {}`作为一个空字典,然后用`grouped[i]`取出字典键值,然后如果键值是`[]`列表类型,那么可以`grouped.append()`直接添加,也可以`grouped.[i]+=[m]`

#迭代器
**Iterators**:
可迭代对象是可以迭代或一次遍历一个元素的任何值。我们用来迭代可迭代对象的一个结构是 for 语句
```python
for elem in iterable
```

通常，可迭代对象是调用内置 `iter` 函数返回迭代器的对象。迭代器是调用内置 `next` 函数返回下一个值的对象

列表式可迭代的值,但是不是可迭代的迭代器

```python
>>> s = [1, 2, 3, 4]
>>> next(s)       # s is iterable, but not an iterator
TypeError: 'list' object is not an iterator
>>> t = iter(s)   # Creates an iterator
>>> t
<list_iterator object ...>
>>> next(t)       # Calling next on an iterator
1
>>> next(t)       # Calling next on the same iterator
2
>>> next(iter(t)) # Calling iter on an iterator returns itself
3
>>> t2 = iter(s)
>>> next(t2)      # Second iterator starts at the beginning of s
1
>>> next(t)       # First iterator is unaffected by second iterator
4
>>> next(t)       # No elements left!
StopIteration
>>> s             # Original iterable is unaffected
[1, 2, 3, 4]
```
next还是会返回可迭代值的数目
```python
>>> z = zip([30, 40, 50], [3, 4, 5])  
>>> next(z)  
(30, 3)  
>>> next(z)  
(40, 4)
```

注意需要处理next的边界情况,没有正确处理迭代器的边界情况。如果迭代器中没有足够的元素，`next(t)`会抛出`StopIteration`异常
```python
def repeated(t, k):

    """Return the first value in iterator t that appears k times in a row,

    calling next on t as few times as possible.

  

    >>> s = iter([10, 9, 10, 9, 9, 10, 8, 8, 8, 7])

    >>> repeated(s, 2)

    9

    >>> t = iter([10, 9, 10, 9, 9, 10, 8, 8, 8, 7])

    >>> repeated(t, 3)

    8

    >>> u = iter([3, 2, 2, 2, 1, 2, 1, 4, 4, 5, 5, 5])

    >>> repeated(u, 3)

    2

    >>> repeated(u, 3)

    5

    >>> v = iter([4, 1, 6, 6, 7, 7, 8, 8, 2, 2, 2, 5])

    >>> repeated(v, 3)

    2

    """

    assert k > 1

    "*** YOUR CODE HERE ***"

    prev = next(t)

    try:

        k1 = k

        while True:

            current = next(t)

            if prev == current:

                k1 -= 1

                if k1 == 1:

                    return prev

            else:

                k1 = k  # 重置k1，因为当前元素与前一个不同

                prev = current

    except StopIteration:

        pass  # 当迭代器耗尽时退出循环

    return prev
```
使用`try...except`捕获`StopIteration`异常，确保迭代器耗尽时能够安全退出循环


注意
```
s = [6, 7, 8]  
print(s.append(6))
```
打印的是返回值

## lab06

```python
    """Return a list of coins that sum to amount, preferring the smallest coins
    available and placing the smallest coins first in the returned list.
    The coins argument is a dictionary with keys that are positive integer
    denominations and values that are positive integer coin counts.
    >>> make_change(2, {2: 1})
    [2]
    >>> make_change(2, {1: 2, 2: 1})
    [1, 1]
    >>> make_change(4, {1: 2, 2: 1})
    [1, 1, 2]
    >>> make_change(4, {2: 1}) == None
    True
    >>> coins = {2: 2, 3: 2, 4: 3, 5: 1}
    >>> make_change(4, coins)
    [2, 2]
    >>> make_change(8, coins)
    [2, 2, 4]
    >>> make_change(25, coins)
    [2, 3, 3, 4, 4, 4, 5]
    >>> coins[8] = 1
    >>> make_change(25, coins)
    [2, 2, 4, 4, 5, 8]
    """
```
题干:实现 `make_change`，它接受一个正整数 `amount` 和一个字典 `coins`。`coins` 字典键是正整数面额，其值是正整数硬币计数。例如，`{1: 4, 5: 2}` 表示 4 便士和 2 镍币。`make_change` 函数返回总和 `amount` 的硬币列表，其中返回值中任何面额 `k` 的计数最多为 `coins[k]`

#递归 #多个情况下进行递归

```python
def make_change(amount, coins):

    if not coins:

        return None

    smallest = min(coins)

    rest = remove_one(coins, smallest)

    if amount < smallest:

        return None

    "*** YOUR CODE HERE ***"

    if amount == smallest:

        return [smallest]

    with_smallest = make_change(amount - smallest, rest)

    if with_smallest is not None:

        return [smallest] + with_smallest

    return make_change(amount, rest)
```

能减smallest的时候(返回不是None),此时返回的是` [smallest] + with_smallest`

不能减去`smallest`的时候,也就是不能用这么多smallest的硬币,所以返回`make_change(amount, rest)`



CS61A给出的解析
1. 检查 if `amount == smallest`，在这种情况下，返回仅包含 `smallest` 的单元素列表
2. 否则，调用 `make_change(amount-smallest, rest)`，这将返回 `None` 或数字列表
3. 如果步骤 2 中的调用返回一个列表，则返回一个较长的列表，该列表的前面包含 `smallest`
4. 如果步骤 2 中的调用返回 `None`，则无法使用 `smallest` 硬币，因此只需 `make_change(amount, rest)`

这个逻辑已经很清晰了

---
补充remove_one函数
```python
def remove_one(coins, coin):
    """Remove one coin from a dictionary of coins. Return a new dictionary,
    leaving the original dictionary coins unchanged.

    >>> coins = {2: 5, 3: 2, 6: 1}
    >>> remove_one(coins, 2) == {2: 4, 3: 2, 6: 1}
    True
    >>> remove_one(coins, 6) == {2: 5, 3: 2}
    True
    >>> coins == {2: 5, 3: 2, 6: 1} # Unchanged
    True
    """
    copy = dict(coins)
    count = copy.pop(coin) - 1  # The coin denomination is removed
    if count:
        copy[coin] = count      # The coin denomination is added back
    return copy
```



### 下一题
案例:完成 `ChangeMachine` 类的 `change` 方法。一个 `ChangeMachine` 实例包含一些 `coins`，这些硬币最初都是1便士。`change` 方法采用一个正整数 `coin`，将该硬币添加到其 `coins` 中，然后返回一个总和 `coin` 的列表。机器更喜欢返回尽可能多的可用最小硬币，从最小到最大排序。`change` 返回的硬币将从机器的 `coins` 中删除
```python
"""A change machine holds a certain number of coins, initially all pennies.
    The change method adds a single coin of some denomination X and returns a
    list of coins that sums to X. The machine prefers to return the smallest
    coins available. The total value in the machine never changes, and it can
    always make change for any coin (perhaps by returning the coin passed in).
    The coins attribute is a dictionary with keys that are positive integer
    denominations and values that are positive integer coin counts.
    >>> m = ChangeMachine(2)
    >>> m.coins == {1: 2}
    True
    >>> m.change(2)
    [1, 1]
    >>> m.coins == {2: 1}
    True
    >>> m.change(2)
    [2]
    >>> m.coins == {2: 1}
    True
    >>> m.change(3)
    [3]
    >>> m.coins == {2: 1}
    True
    >>> m = ChangeMachine(10) # 10 pennies
    >>> m.coins == {1: 10}
    True
    >>> m.change(5) # takes a nickel & returns 5 pennies
    [1, 1, 1, 1, 1]
    >>> m.coins == {1: 5, 5: 1} # 5 pennies & a nickel remain
    True
    >>> m.change(3)
    [1, 1, 1]
    >>> m.coins == {1: 2, 3: 1, 5: 1}
    True
    >>> m.change(2)
    [1, 1]
    >>> m.change(2) # not enough 1's remaining; return a 2
    [2]
    >>> m.coins == {2: 1, 3: 1, 5: 1}
    True
    >>> m.change(8) # cannot use the 2 to make 8, so use 3 & 5
    [3, 5]
    >>> m.coins == {2: 1, 8: 1}
    True
    >>> m.change(1) # return the penny passed in (it's the smallest)
    [1]
    >>> m.change(9) # return the 9 passed in (no change possible)
    [9]
    >>> m.coins == {2: 1, 8: 1}
    True
    >>> m.change(10)
    [2, 8]
    >>> m.coins == {10: 1}
    True
    >>> m = ChangeMachine(9)
    >>> [m.change(k) for k in [2, 2, 3]]
    [[1, 1], [1, 1], [1, 1, 1]]
    >>> m.coins == {1: 2, 2: 2, 3: 1}
    True
    >>> m.change(5) # Prefers [1, 1, 3] to [1, 2, 2] (more pennies)
    [1, 1, 3]
    >>> m.change(7)
    [2, 5]
    >>> m.coins == {2: 1, 7: 1}
    True
    """
```

```python
class ChangeMachine:
    def __init__(self, pennies):
        self.coins = {1: pennies}
    def change(self, coin):
        """Return change for coin, removing the result from self.coins."""
        "*** YOUR CODE HERE ***"
        self.coins[coin] = 1 + self.coins.get(coin, 0)  # Put the coin in the machine
        result = make_change(coin, self.coins)
        for c in result:
            self.coins = remove_one(self.coins, c)
        return result
```

为什么这样写,copilot的回答

这里专门解释了m.change(3)的情况

假设 `m` 是一个 ChangeMachine 对象，并且它的 coins 属性初始状态如下：
`m.coins = {2: 1}`

调用 `m.change(3)` 时，程序的执行过程如下
1. **将硬币放入机器**
```python
self.coins[coin] = 1 + self.coins.get(coin, 0)
```
这里coin是3,所以`self.coin.get(3.0)`返回0,然后`1+0 = 1`,所以`self.coins[3]`被更新成1,此时`self.coins`变为
```python
{2:1,3:1}
```

2.  **调用`make_change`函数**
```python
result = make_change(coin, self.coins)
```

这里`coin`是3,`self.coins`是 `{2: 1, 3: 1}`,`make_change`函数的目的是找到一组硬币,使得他们的总和等于3,并且有限使用面值最小的硬币
在`make_change`函数中
	1. 找到最小的硬币面值为2
	2. 递归调用`make_change`函数,`make_change(1,{2:0,3:1})`
		1. 找到最小的硬币面值为3
		2. 递归调用`make_change`,返回None
		3. 递归调用`make_change(1, {2: 0, 3: 1}`,返回 `None`
	 3. 递归调用`make_change`
	 4. ...省略,反正此时返回都是None
 
3. 更新了self.coins
```python
for c in result:
    self.coins = remove_one(self.coins, c)
```


4. 返回结果:

```python
return result
```

返回 `[3]`

最终，`m.change(3)` 返回 `[3]`，并且 `m.coins` 变为 `{2: 1}`