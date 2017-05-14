title: Python和C++里面计算对数和开方
date: 2017-05-02 21:15:36
categories: 我学编程
tags: 
 - Mathematics
mathjax: true
toc: true
comments: true
---

## 对数
&emsp;&emsp;一般地，如果$a^x=N(a>0, 且a\neq1)$ ,那么数$x$叫做以$a$为底$N$的**对数(logarithm)**，记做

$$x=log_aN$$

其中$a$叫做对数的底数，$N$叫做真数。
&emsp;&emsp;通常我们将以10为底的对数叫做**常用对数**，并把$log_{10}N$记做$lgN$.另外，在科学技术中常使用以无理数$e=2.71828...$为底数的对数，以$e$为底的对数称为**自然对数**，并且把$log_eN$记做$lnN$.
&nbsp;&nbsp;&nbsp;&nbsp;根据对数的定义，可以得到对数与指数间的关系：
当$a>0,a\neq1$时，$a^x=N \iff x=log_aN$.
由指数与对数的这个关系，可以得到关于对数的如下结论：
负数和零没有对数。
$log_a1=0, log_aa=1$.

例子：

$log_{2}4=2$ 

$log_{5}625=5$  

$log_{2}\frac{1}{64}=-6$

## 开方
开方，指求一个数的方根的运算，为乘方的逆运算，若一个数$b$为数$a$的$n$次方根，则$b^n=a$，$\sqrt[n]{a}=b$，读作$a$的$n$次方根等于$b$.
2次方根称为平方根，3次方根称为立方根。
在实数范围内，下式成立：
$$b^n=a$$
- 如果n为偶数，此时$b=\pm \sqrt[n]a$
- 如果n为奇数，此时$b=\sqrt[n]a$

## Python里求对数和开方
```Python
import math

a = math.log(27, 3)  # 求以3为底27的对数
b = math.log2(1024)  # 求以2为底1024的对数
c = math.log10(100)  # 求以10为底100的对数
d = math.log(math.e) # 求以e为底e的对数  e = 2.71828...
e = math.log1p(math.e - 1) # 求以e为底1 + x的对数
print(a, b, c, d, e)

x1 = math.e
x2 = math.pi
x3 = math.sqrt(4)
x4 = math.pow(8, 0.5)
x5 = math.ceil(1.3)
x6 = math.floor(1.7)
x7 = math.trunc(8.39)
x8 = round(math.pi, 4)
print("math.e = ", x1)
print("math.pi = ", x2)
print("math.sqrt(4) = ", x3)
print("math.pow(8, 0.5) = ", x4)
print("math.ceil(1.3) = ", x5)
print("math.floor(1.7) = ", x6)
print("math.trunc(8.39) = ", x7)
print("round(math.pi, 4) = ", x8)
'''
output:
3.0 10.0 2.0 1.0 1.0
math.e =  2.718281828459045
math.pi =  3.141592653589793
math.sqrt(4) =  2.0
math.pow(8, 0.5) =  2.8284271247461903
math.ceil(1.3) =  2
math.floor(1.7) =  1
math.trunc(8.39) =  8
round(math.pi, 4) =  3.1416
'''
```
## C++里求对数和开方
```C++
#include <iostream>
#include <cmath>
using namespace std;


int main() {
    int a = 10;
    int b = 1024;
    int c = 16;
    // 以10为底a的对数
    cout << log10(a) << endl;
    // 以e为底exp(1)的对数，exp(1)为e的1次方
    cout << log(exp(1)) << endl;
    // 以2为底b的对数
    cout << log2(b) << endl;
    // 以4为底16的对数，和Python不同，因为C++中没有log(4, 6)这样的函数，这里用到了换底公式
    cout << log(16) / log(4) << endl;
    // 8的二次方根
    cout << sqrt(8) << endl;
    // 2.0的4.0次方
    cout << pow(2.0, 4.0) << endl;
}
/**
1
1
10
2
2.82843
16
 */
```

