---
title: 面向对象编程
tags:
  - CS61A
  - Python
comments: true
statistics: true
---
## 基本概念

面向对象编程 （OOP） 是一种根据对象和类组织程序的方法。

以下是一些术语：

1. class：对象的类型，用于描述其实例的行为。类 Car （如上所示） 描述所有 Car 对象所具有的行为。
2. 实例：“实例”一词与“对象”的含义相同;每个对象都是其类的一个实例。在 Python 中，通过调用类来创建新实例。
```python
>>> ferrari = Car('red')
```
3. attribute：对象具有命名属性，例如本例中的 wheels 和 color。可以使用 点 表达式 访问它们，并使用 assignment 更改它们
```python
>>> ferrari.color
'red'
>>> ferrari.wheels
4
>>> ferrari.color = 'green'
>>> ferrari.color
'green'
```
4. method：方法是作为类属性的函数，使用点表达式调用。方法通常描述与对象关联的作，例如 drive 汽车。

```python
>>> ferrari = Car('red')
>>> ferrari.drive()
'red car goes vroom!'
```

5. __init__特殊方法：名为 __init__ 的方法很特殊，因为它在创建类的新实例时会自动调用;这是我们初始化实例属性的地方。表达式 Car('red') 调用 Car 类的 `__init__` 方法

6. self：在 Python 中，self 是方法的第一个参数 调用方法时，self 会自动绑定到调用该方法的点表达式中使用的类的实例

```python
>>> ferrari = Car('red')
>>> ferrari.drive()
```

请注意 drive 方法如何将 self 作为参数，但看起来我们没有传入self参数！这是因为点表示法将 ferrari 隐式地传入为 self。因此，在此示例中，self 绑定到全局帧中名为 ferrari 的对象

## Class定义

以银行账号为例子

```python
class Account:
    #定义创建类的新实例的时候回发生什么
    def __init__(self,account_holder):
        self.balance = 0
        self.holder = account_holder
    #必须用特殊的方法名__init__来做到这一点，这些方法总是带有下划线，因为再创建新account时自动调用的函数

    #此时定义了属性，还没有定义行为
    def deposit(self,amount)
        self.balance = self.balance + amount
        return self.balance
        #在__init__中定义的新属性已经可以使用了
    def wihdraw(self,amount)
        self.balance = self.balance - amount
        return self.balance
```


我们在调用的时候不需要输入self

属性的值可以是另外一个对象，也可以创建不同的属性，会自动命名

```python
#例如
>>>b = Account('jimmy')
>>>a = Account('butler')
>>>a.backup = b
#我们给a赋予了新属性，那么我们可以这样调用
>>>a.backup.balance = 0
0
```

### 对象识别

对象的值可以改变，所以对象可以变异。但是必须知道这是两个属性相同但是实际上不同的对象，还是两个相同的对象

```python
>>>a = Account('jimmy')
>>>b = Account('Johh')
>>>a is a
True
>>>a is not b
True
```

我们调用了account两次，所以申明了两个对象

如果我们
```python
>>>a = Account('jimmy')
>>>c = a
```

此时就是两个一样的对象

## 方法定义

所有的调用方法都可以用self访问对象，他们都可以访问和操作对象的属性

```python
def deposit(self,amount)
    self.balance = self.balance + amount
    return self.balance
```

这个deposit是用于account的方法，不能用在其他种类的对象上

### 类属性

执行类语句的时候，会创建一个新类

在类语句的套件内部，赋值语句和def语句创建类属性

如果所有的对象都有相同的属性，那么我们可以用类属性来表示，而不是在每个对象中都使用实例属性，只需要在类中存储一次


```python
class Account:
    interest = 0.02
    #定义创建类的新实例的时候回发生什么
    def __init__(self,account_holder):
        self.balance = 0
        self.holder = account_holder
    #必须用特殊的方法名__init__来做到这一点，这些方法总是带有下划线，因为再创建新account时自动调用的函数

    #此时定义了属性，还没有定义行为
    def deposit(self,amount)
        self.balance = self.balance + amount
        return self.balance
        #在__init__中定义的新属性已经可以使用了
    def wihdraw(self,amount)
        self.balance = self.balance - amount
        return self.balance
```

此时我们可以这样调用

```python
>>>a = Account('jimmy')
>>>a.interest
0.03
```

实例和他们的类都有属性，都可以用`.`来访问，如果存在该名字的实例属性或者类属性，就返回其值

如果返回了是一个函数，那么就是返回一个绑定了的方法，其中点表达式的对象已经赋值给了self参数

顺序是先实例对象，然后再是类

>`getatter()`可以替代`.`表达式，例如`getattr(tom_account,'balance')`

>`hasatter()`可以判断是否存在这个属性

### 类赋值

```python
Account.interest=0.04
```

这样我们就修改了整个类属性

当然，也可以

```python
jimmy.interest = 0.05
tom.interest = 0.02
```

这样就给不同的账号设置了不同的interest，但是account的interest不受影响，新建的account变量仍然是0.03的interest

并且，此时我们再调整account的interest的时候，不会影响特殊情况，这里的特殊情况指的是我们刚刚修改的account种类的interest

### 调用方法

调用方法是主要的交互方式

注意`__init__`也是一个方法，但是不直接调用，一般再创建class实例的时候自动发生

我们可以把`expression.name`赋值给`function`变量名

例如

```python
class Account:
    interest = 0.02
    def __init__(self,account_holder):
        self.balance = 0
        self.holder = account_holder
    def deposit(self,amount)
        self.balance = self.balance + amount
        return self.balance
    def wihdraw(self,amount)
        self.balance = self.balance - amount
        return self.balance
```


```bash
>>>a = Account('AMY')
>>>f = a.withdraw
>>>g = a.deposit
>>>g(1)
1
>>>f(1)
0
```

f和g都被绑定了一个方法


### 绑定方法

绑定方法是函数，也是类属性，其中self参数已经填充为类的一个实例

对象+函数=绑定方法

```python
>>>type(Account.deposit)
<class 'function'>
>>>type(tom.deposit)
<class 'method'>
```

这就导致了有两种方式来调用函数/绑定方法

```python
>>>Account.deposit(tom,10)
10
>>>tom.deposit(10)
20
```

前一种是函数方法，传递两个函数

另外的是绑定方法，self参数被自动传递

## 继承

这是python对象系统的新特性，也是每一个对象系统和编程语言都存在的特性

继承：是将多个类关联在一起的方法，因为并非每一个类都孤立存在

有时候类与类之间相似，并且在specialization程度上有所不同，这个时候我们考虑继承

专业化的类可能具有与通用类相同的所有属性，以及一些特殊情况的行为

使用继承的时候，我们编写现有类的子类时，只需要说明这个子类和父类有什么不同点，其他保持一致

```python
class <name>(<base name>)
    <suite>
```

```python
class CheckingAccount(Account)
    withdraw_fee = 1
    interest = 0.01
    def withdraw(self,amount)
        return Account.withdraw(self.amount+self.withdrwa_fee)
```

这里self就是我们代之调用此方法的实例

父类的属性不会赋值到子类中，相反，这是按照名称查找属性的一部分，使得子类中未更改的属性和父类中一样

因此，在类中查找名称，会经历一下几个步骤

1. 如果name在类中存在，返回属性值
2. 如果没有，在父类中找，返回属性值
3. 如果还没有，就在父类的父类中找

并且，尽量使用父类中的函数，避免父类函数改变的时候，子类函数没有改变，此时并没有继承关系，可能造成一些难以察觉的bug（函数的封装）

## 面向对象的程序设计

在写继承的时候,最好的方式是用self调用父类的方法

```python
class CheckingAccount(Account)
    withdraw_fee = 1
    interest = 0.01
    def withdraw(self,amount)
        return Account.withdraw(self.amount+self.withdrwa_fee)
```

如果子类有自身特殊的同名属性，那么就可以用self调用,如果没有,就用父类的属性

---

**在设计面向对象编程的时候,另外一件值得考虑的事情是何时使用继承而不是使用组合**:

继承(inheritance)适合于子类是父类的特殊情况的情况,表示的是一种is-a的关系

组合(compositon)适合于子类是父类的一部分的情况,表示的是一种has-a的关系

组合是一个对象把另外一个对象当做属性,而不是继承

我们把bank作为一个对象,has-a account,而不是bank is-a account

例如

```python
class Bank:
    def __init__(self):
        self.accounts = []
    def open_account(self,holder,amount,kind=Account):
        """
        kind是一个函数,用于创建一个新的account.这个函数应该接受一个字符串,并且返回一个新的account,默认是Account,但是也可以是CheckingAccount
        """
        account = kind(holder)
        account.deposit(amount)
        self.accounts.append(account)
        #把新的account加入到bank的account列表中
        return account
    def pay_interest(self):
        for account in self.accounts:
            account.deposit(account.balance*account.interest)
    def too_big_to_fail(self):
        return len(self.accounts)>1
```

这样我们就可以创建一个新的account,并且把他加入到bank的account列表中

```python
>>>bank = Bank()
>>>my_account = bank.open_account('jimmy',10)
>>>my_account.deposit(5)
15
>>>my_account.balance
15
>>>bank.pay_interest()
>>>my_account.balance
15.15
```

## 强化属性查找和继承

```python
class A:
    z = -1
    def f(self,x):
        return B(x-1)
class B(A):
    n = 4
    def __init__(self,y):
        if y:
            self.z = self.f(y)
        else:
            self.z = C(y+1)
class C(B):
    def f(self,x):
        return x
```

这里就是很明显的继承关系

C继承B，B继承A

```python
>>>C(2).n
```

此时C(2)调用的是C的类,但是C没有`__init__`方法，所以会调用B的`__init__`方法

也就是创建了一个C的实例，但是调用的是B的`__init__`方法

因为传入的y=2,所以self.z=self.f(2),此时调用的是C的f方法,而不是A的f方法

此时我们返回的是x,也就是2,此时C(2).z=2

因为我们取出的是n属性,所以这个例子中返回的是4

---

```python
>>>C.z == a.z
```

这是`True`，因为C没有z属性，所以会调用A的z属性

---

## 多重继承

一个类可以从多个类继承

```python
class A:
    def f(self):
        return 2
class B:
    def f(self):
        return 3
class C(A,B):
    def g(self):
        return self.f()
```

这里C继承了A和B，但是A和B都有f方法，此时调用的是A的f方法

```python
>>>c = C()
>>>c.f()
2
```

---

多重继承的时候，如果有多个父类有相同的方法，那么调用的时候，会按照继承的顺序来调用

如果父类又不同的方法,那么不同的方法会被`.`调用

---

## 字符串表示

在python中,所有对象都有两种字符串表示,一种是`str`，一种是`repr`

`str`是用于用户的字符串表示，`repr`是用于解释器的字符串表示,`str`是人类可读的，`repr`是解释器可读的

通常情况下，`str`和`repr`是一样的，但是有时候会不一样

首先我们考虑repr

```python
class Account:
    def __init__(self,account_holder):
        self.balance = 0
        self.holder = account_holder
    def deposit(self,amount)
        self.balance = self.balance + amount
        return self.balance
    def wihdraw(self,amount)
        self.balance = self.balance - amount
        return self.balance
    def __repr__(self):
        return 'Account({})'.format(self.holder)
```

返回对象的规范字符串表示

调用eval函数，可以返回一个对象

```python
>>>12e12
120000000000.0
>>>eval(repr(12e12))
120000000000.0
```

---

```python
>>>from fractions import Fraction
>>>half = Fraction(1,2)
>>>half
Fraction(1,2)
>>>repr(half)
'Fraction(1,2)'
>>>eval(repr(half))
Fraction(1,2)
>>>str(half)
'1/2'
>>>eval(str(half))
0.5
```

>用eval函数打印str(half)得到的是0.5，这一个浮点数,不是分数.使用1/2是为了方便人类阅读,明白这是一个分数

str是一个内置函数,接受任何对象,返回一个字符串.其中字符串是人类可读的

对表达式调用str的结果是实际调用print函数的时候会打印的结果

```python
>>>print(half)
1/2
```

---

repr和str的区别

```python
>>>s = 'hello'
>>>s
'hello'
>>>repr(s)
"'hello'"
>>>str(s)
'hello'
>>>print(s)
hello
>>>print(repr(s))
'hello'
>>>print(str(s))
hello
>>>eval(repr(s))
'hello'
>>>repr(repr(repr(s)))
'\'"\\\'hello\\\'"\'' #这是一个字符串,`\`是转义字符
>>>eval(eval(eval(repr(repr(repr(s))))))
'hello'
>>>eval(s)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<string>", line 1, in <module>
NameError: name 'hello' is not defined. Did you mean: 'help'?

```

repr返回的是一个字符串，如果我调用它,进行输出的话,会得到原始字符串

'hello'是一个字符串,不是有效的python表达式,所以eval会报错
如果加上`""`就是一个有效的python表达式,所以eval(repr(s))就不会报错

>`eval(str(s))`会报错,因为str(s)返回的是一个字符串,而不是一个表达式

---

## F-string

f-string是一种字符串格式化的方法,可以在字符串中插入表达式

我们能通过字符串连接做到这一点,但是f-string更加简洁

```python
>>>from math import pi
>>>'pi starts with' + str(pi) +'...'
'pi starts with 3.141592653589793...'
>>>print('pi starts with' + str(pi) + '...')
pi starts with 3.141592653589793...
>>>f'pi starts with {pi}...'
'pi starts with 3.141592653589793...'
>>>print(f'pi starts with {pi}...')
pi starts with 3.141592653589793...
```

f-string是在字符串前面加上f,然后在字符串中用`{}`来插入表达式

f字符串特殊之处在于可以包含大括号内的其他表达式,我们可以在大括号内写任何表达式

```python
>>>f'2+2 = 2+2'
'2+2 = 2+2'
>>>f'2+2 = {2+2}'
'2+2 = 4'
>>>f'2+2 = {2+2} and 3+3 = {3+3}'
'2+2 = 4 and 3+3 = 6'
>>>f'2+2 = {float(2+2)}'
'2+2 = 4.0'
```

---

任何合法的python表达式都可以放在大括号内

F-string表达式的结果是str形式

```python
>>>from fractions import Fraction
>>>half = Fraction(1,2)
>>>half
Fraction(1,2)
>>>print(half)
1/2
>>>f'half is {half}'
'half is 1/2'
>>>f'half is {half} and float(half) is {float(half)}'
'half is 1/2 and float(half) is 0.5'
>>>f'half of half is {repr(half*half)}'
'half of half is Fraction(1,4)' 
```

子表达式的计算和python其他的地方一样,但是可能有副作用

```python
>>>s = [1,2,3]
>>>f'because {s.pop()} {s.pop()} {s}'
'because 3 2 [1]'
```

会导致s的值改变

## 多态函数

多态函数是一个函数,可以接受多种类型的参数,并且根据参数的类型,返回不同的结果

例如str和repr函数,通过特殊的方法名`__str__`和`__repr__`来实现

封装只调用一个名为`__str__`或者`__repr__`的方法,然后根据参数的类型,返回封装字符串


```python
>>>half = Fraction(1,2)
>>>half.__repr__()
'Fraction(1,2)'
>>>half.__str__()
'1/2'
```

---

如何实现多态函数

```python
class Ratio:
    def __init__(self,n,d):
        self.numer = n
        self.denom = d
    def __repr__(self):
        return f'Ratio({self.numer},{self.denom})'
    def __str__(self):
        return f'{self.numer}/{self.denom}'
```

```python
>>>half = Ratio(1,2)
>>>half
Ratio(1,2)
>>>print(half)
1/2
```

---

## 特殊方法

在python中,有一些特殊的方法,他们的名字以`__`开始和结束,这些方法是python解释器调用的,而不是我们直接调用的

例如`__init__`方法,在创建一个新的实例时自动调用

```python
class Ratio:
    def __init__(self,n,d):
        self.numer = n
        self.denom = d
    def __repr__(self):
        return f'Ratio({self.numer},{self.denom})'
    def __str__(self):
        return f'{self.numer}/{self.denom}'
```

`__repr__`和`__str__`方法,在调用`repr`和`str`函数时自动调用

`__add__`方法,在调用`+`运算符时自动调用

`__bool__`方法,将对象转换成布尔值,返回`True`或者`False`,调用bool函数时自动调用

```python
>>>zero , one , two = 0 , 1 , 2
>>>bool(zero),bool(one),bool(two)
(False,True,True)
>>>one.__bool__()
True
>>>zero.__add__(one)
1
>>>one.__add__(two)
3   
```

`__float__`方法,将对象转换成浮点数,调用float函数时自动调用

### add方法和radd方法

`__add__`方法,在调用`+`运算符时自动调用

```python
>>>Ratio(1,3) + Ratio(1,6)
Ratio(1,2)
>>>Ratio(1,3).__add__(Ratio(1,6))
Ratio(1,2)
```

在python中上面两个是等价的

```python
>>>Ratio(1,3).__radd__(Ratio(1,6))
Ratio(1,2)
```

`__radd__`方法,在调用`+`运算符时,如果左边的对象不支持`+`运算符,就会调用`__radd__`方法

用在一些不能加法交换的情况下

```python
class Ratio:
    def __init__(self,n,d):
        self.numer = n
        self.denom = d
    def __repr__(self):
        return f'Ratio({self.numer},{self.denom})'
    def __str__(self):
        return f'{self.numer}/{self.denom}'
    def __add__(self,other):
        if isinstance(other,int):
            n = self.numer + self.denom*other
            d = self.denom
        elif isinstance(other,Ratio):
            n = self.numer*other.denom + other.numer*self.denom
            d = self.denom*other.denom
        elif isinstance(other,float):
            return float(self) + other
            #返回浮点数,调用__float__方法,类型强制转换
    def __float__(self):
        return self.numer/self.denom
        #返回浮点数
    __radd__ = __add__
def gcd(a,b):
    while n != d:
        n,d = min(n,d),abs(n-d)
    return n
```

现在已经实现了将加法添加到用户定义类的接口中
