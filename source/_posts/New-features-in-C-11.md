---
title: New features in C++11
tags: [CPP]
categories: [CPP,C++11]
date: 2019-01-28 17:20:59
description:
---
# Types
## 添加自动检测类型`auto`。

- Syntax
```c++
// Avoiding double writing on pointers
auto pointer = new SomeNamespace::MyLongType(arg1, arg2);

// Does anyone know the type of an iterator? It's ugly!
auto iterator = some_vector.begin();

// This is useful for prototyping, but probably should be explictly typed in real code to help coders
auto type = function_returns_something();

// Worst use of auto; don't do this
auto a = 1;
```

<!-- more -->

## 空指针nullptr

空指针替代关键字`NULL`。
`NULL`等同于`zero`，可能造成类型错误;
`nullptr`不等同于`zero`。

# Containers and iterators
> C++语言在C++11集成迭代器。

- Syntax
```c++
for(vector<double>::iterator iter = vector.begin(); iter != vector.end(); ++iter)
    *value = 0.0;

for(auto &value : vector)
    value = 0.0;
```
类似与传引用`&`将地址赋予value，便于保存内存拷贝和数值修改。

## container constructors
```c++
std::vector<int> values = {1,2,3,4,5,6};
```

# Functional programming

## lambda function
> allows an inline function definition.

- Syntax
```c++
// [](){}
auto square = [](double x){return x*x;};
double squared_five = square(5.0);
```


------------------------------
# References

- [New features in C++11 for LHCb Physicists](https://lhcb.github.io/developkit-lessons/DevelopKit/code/NewCpp/DevelopKit/code/NewCpp/cpp11/for.cpp)
