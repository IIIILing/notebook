---
title: scheme
tags:
  - CS61A
  - Python
statistics: true
comments: true
---
这是CS61A的笔记,和之前的scheme笔记有不同之处

## 基础知识

`(+ 1 2)`的方式来组合,这个python的语法不同

`(quotient 10 2)`得到5

可以使用缩进使得分割任意行,这也是使用缩进使得表达式更加清晰的典型方式

scheme解释器不关心缩进,这只是我们为了使得程序人类可读的方式

重要的是要把括号闭合

判断语法
```scheme
scm>(number? 3)
#t ;表示这是真
scm>(zero? 0)
#t
scm>(zero? 2)
#f
scm>(interger? 2)
#t;判断是否是整数
```

## Scheme解释器

在project4中,我们可以构建一个实现Scheme解释器的python程序

## 特殊语法

`if <judge><case true><case false>`
`and<e1><e2>...<e3>`
`define pi 3.14`
在全局绑定
`define (<symbol><formal paramaters>)<body>`
symbol是名字的形式,formal parameter是形参形式,body才是我们定义的函数主题

定义sqrt函数
```scheme
(define (sqrt x)
		(define (update guess)
		(if (= (square guess) x)
			guess
			(update (average guess (/ x guess)))
		)
	)
	(update 1)
)
```

python的实现
```python
guess = 1  
def sqrt_my(x):  
    def update(guess):  
        if abs(guess*guess - x)<0.000000000000001:  
            return guess  
        else:  
             return update((guess+(x/guess))/2)  
    return update(guess)
```

## lambda表达式

lambda表达式在scheme中非常重要

`(define plus4)(lambda (x) x+4)`=`(define (plus4 x)(+ 4 x))`

可以直接定义plus4,也可以使用lambda表达式定义

## 其他形式

if-elif的表达形式在scheme中要用cond实现

```scheme
cond(
	((> x 10)(print 'big))
	((= x 10)(print 'medium))
	(else (print 'small))
)
```
或者
```scheme
print(
(cond
	((> x 10) 'big)
	((= x 10) 'medium)
	((< x 10) 'small)	
	)
)
```

## Let

let只是临时将一个值绑定到别的式子上

```scheme
(define c 
	(let
		((a 3)
		(b 4))
		(sqrt (+ (* a a) (* b b))
	)
)
```

用let跟踪一点临时元素


## Scheme Lists

在scheme中列表就像python中的链表

cons产生两个元素,用来创建一个链表

car返回列表的第一个元素,cdr返回列表的其余部分

还有一个nil内置符号,用来表示空列表

schemelist显示未括号,其中的元素用空格分割,这是一个基础的链表结构

`(cons 1(cons 2 nil))`-->(1,2)

```scheme
> (define x (cons 1(cons 2 nil)))
> x
(1 2)
>(car x)
1
>(cdr x)
(2)
>(cons 1 (cons 2 (cons 3 (cons (4 nil))))
(1 2 3 4)
```

可以使用car和cdr来访问这些元素

在cs61a的web解释器中,我们可以使用(draw s)来画出链表

```scheme
scm>(define s(cons 1(2 nil)))
(1 2)
scm>(cons s(cons s nil))
((1 2) (1 2))
```
指向了一个s两次,可以draw一下

我们可以用`(list? s)`来判断是否是scheme的列表

`(list? nil)`返回的是`#t`

如何判断是不是空列表

`(null? s)`返回`#f`

但是`(null? nil)`返回的是`#t`

## 符号式编程

一个简单的例子理解

```scheme
>(define a 2)
>(define b 1)
>(list a b)
(2 1)
```

这样list里的仅仅是一个值,不是ab,我们改变ab此时list里的值不会改变

但是我们可以这样写,让list里传入的是数字而不是值

```scheme
>(scheme a 3)
>(scheme b 3)
>(list 'a 'b)
(a b)
>(list 'a b)
(a 3)
```

这引用其实是成为quote的特殊形式的简写

## 内置的列表处理

`(append s t)`可以把多个元素加到s的末尾

`(map f s)`调用一个程序`f`,对s中的每一个都处理过去,最后以list的方式返回结果

`(filter f s)`调用f在每一个list的,如果返回true的元素会在结果list中返回

`(apply f s)`

