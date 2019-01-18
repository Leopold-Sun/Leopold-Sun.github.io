---
title: C++11 Program Structure
tags: [CPP]
categories: [CPP,C++11]
date: 2019-01-18 16:01:53
description:
---
# [Statements and Flow Control](http://www.cplusplus.com/doc/tutorial/control/)

> statements/语句
C++语句即是程序中的单个的指令，以分号(';'semicolon)结束在程序执行时按顺序执行。Flow control控制、执行语句。

<!-- more -->

## Statement Structure

| **Statement** | **syntax** | **Description** |
| :----: | :---- | :---- |
| Selection statement:<br> **if and else**, **switch** | **if and else**:<br>if (condition) statement <br> if (condition) statement1 else statement2; **switch**:<br> syntax is written at the bottom of this tab | **if and else**: if and only if the condition is fulfilled can the following statements or block be executed <br> **switch**: check for a value among a number of possible constant expressions. It is something similar to concatenating if-else statements, but limited to constant expressions |
| Iteration statement:<br> **loops**: while, do, for | **the while loop**:<br> while (expression) statement; <br> **the do-while loop**:<br> do statement while (condition); <br> **the for loop**:<br> for (initialization; condition; increase) statement; <br> **Range-based loop**:<br> for (declaration : range) statement; | Loops repeat a statement a certain number of times, or while a condition is fulfilled.<br> **while loop**: The while-loop simply repeats statement while expression is true.<br> **do-while loop**: Comparing with while-loop, the do-while loop guarantees at least one execution of *statement*, even if *condition* is never fulfilled.<br> **for loop**: it repeats statements while *condition* is true.<br> **range-based loop**: *declaration* declares some variable able to take the value of an element in this range, and this loop iterates over all elements in a range.  |
| Jump statements:<br> **break**,**continue**,**goto** | **break**;<br> **continue**;<br> **goto**; | **the break**: the break leaves a loop, even if the condition for its end is not fulfilled. It could be used to force a statement to end rather than loops infinite or end naturally <br> **the continue**:<br> the continue statement for teh program to skip the rest of the loop in the current iteration, causing it to jump to the start of the following iteration <br> **the goto**:<br> *goto* allows an absolute jump to another point in the program. It is a feature to use with care, and preferably within the same block of statements, especially in the presence of local variables.The *destination point* is identified by a <mark>*label*</mark>,which is then used as an argument for the goto statement..A label is made of a valid identifier followed by a colon (:). |

- Switch syntax
```c++
switch (expression)
{
  case constant1:
     group-of-statements-1;
     break;
  case constant2:
     group-of-statements-2;
     break;
  .
  .
  .
  default:
     default-group-of-statements
}
```

- Examples
```c++
//swicth
switch (x) {
  case 1:
  case 2:
  case 3:
    cout << "x is 1, 2 or 3";
    break;
  default:
    cout << "x is not 1, 2 nor 3";
  }

// custom countdown using while loop
#include <iostream>
using namespace std;

int main ()
{
  int n = 10;

  while (n>0) {
    cout << n << ", ";
    --n;
  }

  cout << "liftoff!\n";
}

// echo machine using do-while loop
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str;
  do {
    cout << "Enter text: ";
    getline (cin,str);
    cout << "You entered: " << str << '\n';
  } while (str != "goodbye");
}

// countdown using a for loop
#include <iostream>
using namespace std;

int main ()
{
  for (int n=10; n>0; n--) {
    cout << n << ", ";
  }
  cout << "liftoff!\n";
}

// range-based for loop
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  string str {"Hello!"};
  for (char c : str)
  {
    cout << "[" << c << "]";
  }
  cout << '\n';
}

// break loop example
#include <iostream>
using namespace std;

int main ()
{
  for (int n=10; n>0; n--)
  {
    cout << n << ", ";
    if (n==3)
    {
      cout << "countdown aborted!";
      break;
    }
  }
}

// continue loop example
#include <iostream>
using namespace std;

int main ()
{
  for (int n=10; n>0; n--) {
    if (n==5) continue;
    cout << n << ", ";
  }
  cout << "liftoff!\n";
}

// goto loop example
#include <iostream>
using namespace std;

int main ()
{
  int n=10;
mylabel:
  cout << n << ", ";
  n--;
  if (n>0) goto mylabel;
  cout << "liftoff!\n";
}
```
---------------------------------------------

# Functions 
> C++函数是具有`identifier`的代码块，其他的`statement`可以通过`identifier`调用该函数。

1. syntax

```c++
type identifier ( parameter1, parameter2, ... ) { statements }
```
type类别: char, int, float, pointer, void。`void`表示无类型。

2. Example

```c++
// function example
#include <iostream>
using namespace std;

int addition (int a, int b)
{
  int r;
  r=a+b;
  return r;
}

int main ()
{
  int z;
  z = addition (5,3);
  cout << "The result is " << z;
}
```

3. Return value
```bash
The result is 8
```

## 传值与传引用

> 在被调用（called）函数声明、定义处的参数列表中所定义的变量被称为`形参`;调用语句所传的变量被称为`实参`。而参数传递有两种方式：传值+传引用。

1. by value
表示向被调用函数传递的是参数列表中参数的值，本质是将参数的值复制赋值给函数的形参。
```c++
int function (int a, int b){...}
...
int x=5, y=3, z;
z = function ( x, y );
```

2. by reference
在函数体内部访问其外部的变量。
```c++
// passing parameters by reference
#include <iostream>
using namespace std;

void duplicate (int& a, int& b, int& c)
{
  a*=2;
  b*=2;
  c*=2;
}

int main ()
{
  int x=1, y=3, z=7;
  duplicate (x, y, z);
  cout << "x=" << x << ", y=" << y << ", z=" << z;
  return 0;
}
```
```bash
x=2, y=6, z=14
```

> 从传值与传引用的区别来看，对于字节比较少的类型比如`int`,传值所引入的内存拷贝开销不会很多。但对于组合类型的参数值传递，如`string`，传值所引入的内存开销可能就无法忽视了。而传引用不会引入额外的内存拷贝，因为其传递的相当与是内存地址，或者说是指针型数据。

## 内联函数
> Inline functions
调用函数通常会导致一定的开销（堆栈参数，跳转等等），因此对于非常短的函数，简单地插入调用它的函数的代码可能更有效，而不是执行该过程正式调用函数。<br>
使用`inline`显示通知编译器内联扩展优先与常规函数调用。即是使编译器编译时直接将函数代码在调用点插入编译。<br>
<mar>inline</mark>在函数只在<mark>函数声明</mark>处显示标识。

- Example
```c++
inline string concatenate (const string& a, const string& b)
{
  return a+b;
}
```

## Recursivity
> 递归性：函数调用自身。

- Example
```c++
// factorial calculator
#include <iostream>
using namespace std;

long factorial (long a)
{
  if (a > 1)
   return (a * factorial (a-1));
  else
   return 1;
}

int main ()
{
  long number = 9;
  cout << number << "! = " << factorial (number);
  return 0;
}
```

----------------------------------------------

# Reference

- [C++ 参考手册](https://zh.cppreference.com/w/cpp)
- [C++ Reference](https://en.cppreference.com/w/)
- [New features in C++11 for LHCb Physicists](https://lhcb.github.io/developkit-lessons/first-development-steps/05a-cpp11.html)
