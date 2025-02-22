---
title: 解释器
tags:
  - CS61A
  - Python
comments: true
statistics: true
---

解释器是一个接受编程语言的代码作为输入的程序

然后执行改代码以创建程序描述的行为

scheme语言的规则很少,所以我们可以去用python编写一个描述scheme语言的解释器

解释器的工作方式就是通过树递归

解释器的一部分将描述解释的一般工作原理,另一方面将封装语言的各个部分实际执行的所有细节

## Reading Scheme Lists

scheme的基本数据结构是列表,一般像这样

```scheme
(<element_0><element_1>...<element_n>)
```

