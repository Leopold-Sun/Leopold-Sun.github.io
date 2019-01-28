---
title: C++ Class
tags: [CPP]
categories: [CPP,C++11]
date: 2019-01-23 13:06:52
description:
---

# Classes

`class`类是C++对数据结构的扩展，可以实现自己的数据成员、函数成员。

- Syntax
```c++
class class_name {	//class\_name是该类的标识符。
  access_specifier_1:
    member1;
  access_specifier_2:
    member2;
  ...
} object_names;		//object\_name是使用该类定义的可选对象标识符。
```

<!-- more -->

### calss vs data structures
类比一般的数据结构多了成员函数的特征以及访问特征符(**access specifier**: public, private, or protected)。
```c++
access specifier:	//修改类成员的访问的权限
- private	//只能被同类成员访问
- public	//可以被同类成员或者派生类访问
- protected	//可以被对象所处的空间的所有对象访问
```
> 类成员默认访问权限为`private`。

### 访问类成员
> 使用“dot”<.>访问类的公共成员。

```c++
//类Rectangle的声明
class Rectangle {
    int width, height;	//私有成员无法被外界访问，不过外界可以通过该类的公共成员访问其私有成员
  public:
    void set_values (int,int);
    int area (void);
} rect;

//访问对象Rect的公共成员
rect.set_values (3,4);
myarea = rect.area(); 
```

### 实例说明
```c++
// classes example
#include <iostream>
using namespace std;

//类外成员函数声明
class Rectangle {
    int width, height;
  public:
    void set_values (int,int);
    int area() {return width*height;}
};

//类公共成员函数的定义
void Rectangle::set_values (int x, int y) {	//scope operators"::"为c++命名空间的语义，表明该定义为成员函数的定义
  width = x;
  height = y;
}

int main () {
  Rectangle rect;
  rect.set_values (3,4);
  cout << "area: " << rect.area();
  return 0;
}
```
- Return value
```c++
area: 12
```

### 内联函数
> 在类的声明中定义的成员函数是`inline`的。

### constructor
> 构造函数为防函数在未初始化就被调用时产生不可预测的返回值。

> 在类内实现一个与该类标识符同名的函数，该函数无类型。

- Example
```c++
// example: class constructor
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle (int,int);	//set_value被替换为构造函数
    int area () {return (width*height);}
};

Rectangle::Rectangle (int a, int b) {
  width = a;
  height = b;
}

int main () {
  Rectangle rect (3,4);
  Rectangle rectb (5,6);
  cout << "rect area: " << rect.area() << endl;
  cout << "rectb area: " << rectb.area() << endl;
  return 0;
}
```

#### overloading constructors
> 重载构造

- Example
```c++
// overloading class constructors
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle ();
    Rectangle (int,int);
    int area (void) {return (width*height);}
};

Rectangle::Rectangle () {
  width = 5;
  height = 5;
}

Rectangle::Rectangle (int a, int b) {
  width = a;
  height = b;
}

int main () {
  Rectangle rect (3,4);
  Rectangle rectb;
  cout << "rect area: " << rect.area() << endl;
  cout << "rectb area: " << rectb.area() << endl;
  return 0;
}
```
	- Return value
	```c++
rect area: 12
rectb area: 25  
	```

### 类指针

- Syntax
```c++
class * pointer_to_class;
```

- Example

```c++
// pointer to classes example
#include <iostream>
using namespace std;

class Rectangle {
  int width, height;
public:
  Rectangle(int x, int y) : width(x), height(y) {}
  int area(void) { return width * height; }
};


int main() {
  Rectangle obj (3, 4);
  Rectangle * foo, * bar, * baz;
  foo = &obj;
  bar = new Rectangle (5, 6);
  baz = new Rectangle[2] { {2,5}, {3,6} };
  cout << "obj's area: " << obj.area() << '\n';
  cout << "*foo's area: " << foo->area() << '\n';
  cout << "*bar's area: " << bar->area() << '\n';
  cout << "baz[0]'s area:" << baz[0].area() << '\n';
  cout << "baz[1]'s area:" << baz[1].area() << '\n';       
  delete bar;
  delete[] baz;
  return 0;
}
```

-------------------------------

### Overloading operators

C++可重载的运算符：
```bash
+    -    *    /    =    <    >    +=   -=   *=   /=   <<   >>
<<=  >>=  ==   !=   <=   >=   ++   --   %    &    ^    !    |
~    &=   ^=   |=   &&   ||   %=   []   ()   ,    ->*  ->   new 
delete    new[]     delete[]
```

### this 关键字
> `this`: 表示指向**正在执行其成员函数的对象**的指针，在类的成员函数用于引用对象本身。

- Example
```c++
// example on this
#include <iostream>
using namespace std;

class Dummy {
  public:
    bool isitme (Dummy& param);
};

bool Dummy::isitme (Dummy& param)
{
  if (&param == this) return true;
  else return false;
}

int main () {
  Dummy a;
  Dummy* b = &a;
  if ( b->isitme(a) )
    cout << "yes, &a is b\n";
  return 0;
}
```

	- Return value
	```c++
yes, &a is b
	```

### 类模板
> [class templates](http://www.cplusplus.com/doc/tutorial/templates/):允许类使用模板。

-------------------------

# Special members
> 六类特殊成员：

|Member function	|typical form for class C:| description | 
| :---------: | :-----: | :---- |
|Default constructor	|C::C();| 不含构造函数的类，编译器默认使用default constructor<br>若是class本身含有带确定参数列表的构造函数，则编译器不提供隐式默认构造函数 |
|Destructor	|C::~C();| 析构函数，多用于类的使用结束时释放内存 |
|Copy constructor	|C::C (const C&);| 拷贝构造：向一个对象传递其同类型的对象作为参数 |
|Copy assignment	|C& operator= (const C&);| 拷贝赋值 |
|Move constructor	|C::C (C&&);||
|Move assignment	|C& operator= (C&&);||

-------------------------------
# Friendship and inheritance
<mark>frineds</mark>: 使用关键字`friend`声明的类。类的友元可以访问该类的私有、保护成员。

- Example
```c++
// friend functions
#include <iostream>
using namespace std;

class Rectangle {
    int width, height;
  public:
    Rectangle() {}
    Rectangle (int x, int y) : width(x), height(y) {}
    int area() {return width * height;}
    friend Rectangle duplicate (const Rectangle&);
};

Rectangle duplicate (const Rectangle& param)
{
  Rectangle res;
  res.width = param.width*2;
  res.height = param.height*2;
  return res;
}

int main () {
  Rectangle foo;
  Rectangle bar (2,3);
  foo = duplicate (bar);
  cout << foo.area() << '\n';
  return 0;
}
```
	- Return value
	```c++
24
	```

### friend classes
> 某类的友类（friend classes）可以访问其私有成员。

- Example
```c++
// friend class
#include <iostream>
using namespace std;

class Square;

class Rectangle {
    int width, height;
  public:
    int area ()
      {return (width * height);}
    void convert (Square a);
};

class Square {
  friend class Rectangle;
  private:
    int side;
  public:
    Square (int a) : side(a) {}
};

void Rectangle::convert (Square a) {
  width = a.side;
  height = a.side;
}
  
int main () {
  Rectangle rect;
  Square sqr (4);
  rect.convert(sqr);
  cout << rect.area();
  return 0;
}
```
	- Return value
	```c++
16
	```

### Inheritance between classes
![](/images/c++/inheritance.png)

> 派生类(derived classed)继承基类(base classes)的全部成员，且可以添加自己的成员。

- Example
```c++
// derived classes
#include <iostream>
using namespace std;

class Polygon {
  protected:
    int width, height;
  public:
    void set_values (int a, int b)
      { width=a; height=b;}
 };

class Rectangle: public Polygon {
  public:
    int area ()
      { return width * height; }
 };

class Triangle: public Polygon {
  public:
    int area ()
      { return width * height / 2; }
  };
  
int main () {
  Rectangle rect;
  Triangle trgl;
  rect.set_values (4,5);
  trgl.set_values (4,5);
  cout << rect.area() << '\n';
  cout << trgl.area() << '\n';
  return 0;
}
```

	- Return value
	```c++
20
10
	```


-----------------------------------

# Polymorphism
> [多态](http://www.cplusplus.com/doc/tutorial/polymorphism/)

### 指向基类的指针
> 基类、派生类的指针可以兼容。

- Example
```c++
// pointers to base class
#include <iostream>
using namespace std;

class Polygon {
  protected:
    int width, height;
  public:
    void set_values (int a, int b)
      { width=a; height=b; }
};

class Rectangle: public Polygon {
  public:
    int area()
      { return width*height; }
};

class Triangle: public Polygon {
  public:
    int area()
      { return width*height/2; }
};

int main () {
  Rectangle rect;
  Triangle trgl;
  Polygon * ppoly1 = &rect;
  Polygon * ppoly2 = &trgl;
  ppoly1->set_values (4,5);
  ppoly2->set_values (4,5);
  cout << rect.area() << '\n';
  cout << trgl.area() << '\n';
  return 0;
}
```
	- Return value
	```c++
20
10
	```
因为Rectangle、Triangle是Polygon的派生类，所有指向Polygon的指针也可以指向其派生类。

### Virtual members
> **虚函数**：可以在派生类中被重新定义的成员函数（属基类成员函数）。

- Syntax
`使用关键字 virtual 声明成员函数`

- Example
```c++
// virtual members
#include <iostream>
using namespace std;

class Polygon {
  protected:
    int width, height;
  public:
    void set_values (int a, int b)
      { width=a; height=b; }
    virtual int area ()
      { return 0; }
};

class Rectangle: public Polygon {
  public:
    int area ()
      { return width * height; }
};

class Triangle: public Polygon {
  public:
    int area ()
      { return (width * height / 2); }
};

int main () {
  Rectangle rect;
  Triangle trgl;
  Polygon poly;
  Polygon * ppoly1 = &rect;
  Polygon * ppoly2 = &trgl;
  Polygon * ppoly3 = &poly;
  ppoly1->set_values (4,5);
  ppoly2->set_values (4,5);
  ppoly3->set_values (4,5);
  cout << ppoly1->area() << '\n';
  cout << ppoly2->area() << '\n';
  cout << ppoly3->area() << '\n';
  return 0;
}
```
	- Return value
	```c++
20
10
0
	```

### Abstract base classes
> **抽象基类**：只能作为*基类*使用的类，允许声明纯虚函数(pure virtual functions: 使用"=0"代替虚函数的定义)。

- Example
```c++
// abstract class CPolygon
class Polygon {
  protected:
    int width, height;
  public:
    void set_values (int a, int b)
      { width=a; height=b; }
    virtual int area () =0;
};
```

