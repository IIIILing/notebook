---
title: Scheme初学笔记
tags:
  - Scheme
  - CS61A
comments: true
statistics: True
---

## 目标

学习基本的Scheme语法,完成CS61A的hw和lab

## 安装MIT-Scheme

面相windows用户的教程

在windows系统上有很多免费的scheme实现,在本次学习中我们使用MIT-Scheme

因为MIT-Scheme非常高效并且容易安装,解释器非常快速,还能将程序编译为本地代码

这里使用wsl安装,因为在官网上已经没有了windows的安装包

```bash
sudo apt-get install mit-scheme
```

之后输入`mit-scheme`即可进入scheme的交互式环境

## 将Scheme用作计算器

```scheme
(+ 137 349)
```

这个表达式的值是486

```scheme
;Value: 486
```

一对括号代表了一次计算的步骤,左括号紧跟一个函数的名字,然后是参数

在scheme中大多数的操作符都是参数

这里的函数是`+`,它的参数是`137`和`349`

标记的分隔符是空格(SPACE),制表符(TAB),换行符(NEWLINE),逗号和分号不是分隔符

**求值过程**:符号`+`被求值为加法过程,在前端输入`+`,解释器会返回`;Value: #[arity-dispatched-procedure 12]`,这说明`+`是一个过程

然后对`137`和`349`求值,这两个数值被求值为它们自己

最后,求值`+`过程,这个过程将`137`和`349`相加,得到`486`并且跳出括号

在scheme中,求得的值会跳出括号外,并且在这个值会被打印到前端

函数`+`可以接受任意多的参数,例如

```scheme
(+ 10 20 30 40)
```

### 基本算术操作

函数`exact->inexact` 用于把分数转换为浮点数。Scheme也可以处理复数。复数是形如a+bi的数，此处a称为实部，b称为虚部。+、-、*和/分别代表加、减、乘、除。这些函数都接受任意多的参数

注意

```scheme
1 ]=> (/ 29 3)

;Value: 29/3

1 ]=> (/ 29 3 7)

;Value: 29/21
```

这些由括号,标记(token)和分隔符组成的式子,被称为S-表达式

其他计算,`quotient`是整数除法,`remainder`是取余数,`sqrt`用于求平方根

```scheme
(quotient 7 3) ;→ 2
(modulo 7 3)   ;→ 1
(sqrt 8)       ;→ 2.8284271247461903
```

三角函数

atan接受1个或2个参数。如果atan的参数为1/2 π,那么就要使用两个参数来计算。

```scheme
(atan 1) ;→ 0.7853981633974483
(atan 1 0) ;→ 0.7853981633974483
```

sin,cos,tan都可以正常使用

对数和指数函数:指数函数exp,对数函数log,log10

```scheme
(exp 1) ;→ 2.718281828459045
(log 2.718281828459045) ;→ 1.0
(log10 1000) ;→ 3.0
(expt a b) ;→ a^b
```

## 生成表

表在递归函数和高阶函数中扮演着重要的角色

### Cons单元和表

Cons单元室一个存放了两个地址的内存空间,Cons单元可以用函数cons生成

在前端输入`(cons 1 2)`

```scheme
;Value: (1 . 2)
```
