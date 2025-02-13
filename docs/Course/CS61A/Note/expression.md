---
title: 表达式
tags:
  - CS61A
  - Python
comments: true
statistics: true
---
## 使用函数作为表达式

所有的表达式都可以使用函数调用符号

>在cmd中输入`python`启动py

`>>>`的地方使我们的表达式,下面一行就是输出的值

演示了两个min,max的py内置函数来表示如何调用表达式

还演示了如何导入模块,`form operator import add,mul`

```python
mul(add(2,mul(4,6)),add(3,5))
```

从内到外调用函数,把参数一个一个算出来

>重点是表达式的执行顺序

## 定义变量/函数

在python中赋值操作可以给函数赋予一个新名字

def用来自定义函数

```python
def square(x):
	return mul(x,x)
```
这里有一个变量,也可以定义没有内置变量的

```python
def area():
	return pi*radius*radius
```
其中这里的radius我们已经定义过了

### 定义函数的细节

函数是更加强大的抽象方式

如何定义函数

```python
def <name>(<formal paramenter>):
	return <return expression>
```

注意要缩进

此时没有任何函数被执行,只是定义了一个函数

当我们调用了函数的时候
1. 产生一个本地框架,形成了新环境
2. 把函数的格式化参数和变量绑定在一起
3. 在新环境中执行函数的主体

注意我们import 一个模块之后,我们就在全局导入了这个环境,我们可以使用这个模块下的所有函数

## None

Python的None相当于C的NULL

都表示没有返回

返回None时,python不会显示任何内容

```python
Print(print(1),print(2))
```

```cmd
1
2
None None
```

因为print函数成功打印之后返回值是None

## 运算

两种除法
`truediv=/`,`floordiv=//`

>`truediv`需要导入operator模块

## 模拟cmd输入

如何执行一个py文件

```python
python main.py
```

执行了之后,如果有打印的信息,会显示在shell上

如果想看更多输出,可以传递`-v`选项

`>>>`是模拟python3的命令行输入,用在注释里

如果用`>>>`写的例子错了.那么在shell也会出现报错

这是通过在特定文件上调用文档测试模块来运行的

	这在写作业时非常好用


在定义函数时,还可以预先设定输入变量的值,如果没有输入,那么就调用预定值

## 条件判断

基本的条件判断语句

```python
<header>:
	<statement>
	<statement>
<separating header>
	<statement>
	<statement>
	<statement>
```

输入`python -i ex.py`可以启动交互式界面,调用出`>>>`来执行这些语句

`if( c, t, f);`三目运算符
和下面这个不同
```python
if c
	t
else
	f
```
t会先被执行(在函数调用前,所有的部分都会被执行),再进行判断是否成为结果输出
## while

```python
while i < 3:
	sum+=i
	i=i+1
```
注意什么时候写冒号,什么时候不写

## 位运算符

and or和C没什么区别

and先执行左边,再执行右边,如果左边为0,那么右边永远不会被计算

or同理,左边如果被执行为1,那么右边永远不会被执行

>注意我们在执行python文件的时候,如果是-i模式,那么我们不需要写input,我们在cmd的输入会被当做交互式输入的输入

如果我们运行py程序的时候,使用的是
```python
python -m doctest main.py
```
会测试我们的例子,如果没有报错没说明结果正确

```python
python -m doctest -i main.py
```
会显示doctest的判断过程

## 函数返回值

函数的返回值就是函数调用后的值

比如我们调用print函数之后并且顺利打印

print()函数现在的值是None

本质上的变量是可以绑定到值的一个名称,这个值可以是数字或者字母,也可以作为函数.

因为函数可以接受任何种类的参数的传入,其他函数可以作为参数传入,这就是高阶函数的基础

## 把函数作为参数

函数对象是可以传递的值,函数作为可以传递的值

因为在python里声明函数我们只需要`def 函数名(函数参数名)`

不需要写出函数的类型,所以有时候一下子看不出来传入的参数是不是函数类型的

当然,我们也可以传入一个lambda表达式

## 返回函数的参数

因为函数也是一种值(values),所以把函数作为返回值是有效的
```python
def multiply_by(m):
    def multiply(n):
        return n * m
    return multiply
```

在这个特殊情况下，我们在 `multiply_by` 的主体中定义了函数 `multiply`，然后返回它。

注意,这个`multiply`不能直接被调用,它只存在于`multiply_by`之下

并且因为`multiply_by`返回的是一个函数,我们可以想起他函数一样调用它的返回值

比如直接`multiply_by(m)(n)`,m传入给`multiply_by`,n传入给内部的`multiply`

## Lambda表达式

Lambda 表达式是通过指定两项内容（参数和返回表达式）来计算函数的表达式

```python
lambda <parameters>: <return expression>
```
`lambda` 表达式的工作方式与其他表达式类似;就像数学表达式只计算为一个数字而不改变当前环境一样，`lambda` 表达式的计算结果为函数而不改变当前环境

def会创建一个新环境,但是lambda不会,可以等同于一个数学表达式,不会受到环境的限制

```python
# We can assign lambda functions to a name  
# with an assignment statement  
square = lambda x: x * x  
square(3)

# Lambda expressions can be used as an operator 
# or operand  
negate = lambda f, x: -f(x)  
negate(lambda x: x * x, 3)
```


