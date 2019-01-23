---
title: C++ data structure & compound data types
tags: [CPP]
categories: [CPP,C++11]
date: 2019-01-21 10:46:23
description:
---

# Contaier - Array
> 数组： 一组类型一致的元素存放在连续的内存位置（contigous memory locations），能够通过给特定的标识符添加指针以便引用该内存地址的值。C++规定，数组第一位被标志为“第0位”。

- Syntax

	- Declaration
	> 数组在被使用前需要被声明。

	C++:
	```c++
#include<array>
array <type,elements> array_identifier {value_1, value_2,...,value_(elements-1)};
	```
	C:
	```c
	type name [elements];
	```
	type: 类型如 float, int, double, bool...
	name: 有效的标识符
	elements: 元素区表示/设定了数组的长度（数组元素的个数），即方括号（square brackets）区域表示了数组的元素数。
	> 数组长度在声明时确定，必须是常数/常数表达式，因为数组被存入静态内存块中，所以在编译时需要确定数组的长度。

	- Definition

	```c++
	type identifier [elements] { value_0, value_1, value_(elements-1) }
	```

<!-- more -->

- 相关操作

	1. 初始化
	> 局部数组不需要初始化，局部数组声明时元素状态为未定义。但可以显式定义数组元素，且在方括号（brackets）中定义数组元素的值。

	`type identifier [elements] = { value_0, value_1, value_(elements-1) }`. 方括号中的数值数不应当超出数组长度，缺省的数组元素（若是基础类型）被默认定义为`zeroes`。而对于`[]`缺省（数组长度为定义），则默认定义的数值数为数组的长度。随着c++的发展，数组声明语法省去了等号（equal sign）`=`。数组声明语法如下：
	```c++
	type identifier [elements] { value_0, value_1, value_(elements-1) }
	```

	2. 数组元素访问
	> 使用`square brackets`指定具体数组元素的位置——索引。方括号的两个作用分别是：其一，声明数组长度;其二，为数组的具体元素提供索引（偏移量）。

	```c++
	name[index]
	```

	3. 多维数组（multidimension arrays）
	> 可以将多维数组描述为`数组的数组`。但是内存的消耗随数组维度的线性增长而呈指数上升，比如多维数组`char century [100][365][24][60][60];`消耗<mark>3 Gigabytes memory</mark>。

	[Example](http://www.cplusplus.com/doc/tutorial/arrays/):
	```c++
	int jimmy [3][5];	// jimmy[1][3]表示第“2”个元素数组的第“4”个元素int型数值。
	```

	4. 数组作为参数传递
	> C++表示，不可能将数组表示的整个内存块作为`argument`直接传递给某个参数。而**`address`**（数组的内存地址）可以作为函数的参数传递，而且这种方式更为高效、快捷。
	以数组为参数的函数声明如下：

	```c++
	type function_identifier (int arg[]);	//arg的类型为 array of int
	```
	Example：
	```c++
// arrays as parameters
#include <iostream>
using namespace std;

void printarray (int arg[], int length) {
  for (int n=0; n<length; ++n)
    cout << arg[n] << ' ';
  cout << '\n';
}

int main ()
{
  int firstarray[] = {5, 10, 15};
  int secondarray[] = {2, 4, 6, 8, 10};
  printarray (firstarray,3);
  printarray (secondarray,5);
}
	```
	Return value:
	```c++
5 10 15
2 4 6 8 10
	```

	5. C++ 数组库(Library arrays)

	> C++提供了标准容器——数组（standard container），该标准库将数组内存块的地址复制退化为`指针操作`。

	C语言内嵌数组：
	```c++
#include <iostream>

using namespace std;

int main()
{
  int myarray[3] = {10,20,30};

  for (int i=0; i<3; ++i)
    ++myarray[i];

  for (int elem : myarray)
    cout << elem << '\n';
}
	```
	数组容器库：
	```c++
#include <iostream>
#include <array>
using namespace std;

int main()
{
  array<int,3> myarray {10,20,30};

  for (int i=0; i<myarray.size(); ++i)
    ++myarray[i];

  for (int elem : myarray)
    cout << elem << '\n';
}
	```
------------------------
# Character sequences
> C++ string 类是一种表示字符串的方式。除此之外，也可以用数组的形式表示字符串。
C++约定，字符串终止符为空字符`the null character`，数值为`\0`(backslash,zero),"\0"表示字符串的终止。比如：

|t|e|s|t|\0||
|-|-|-|-|-|-|

|H|e|l|l|o|\0||||||
|-|-|-|-|-|-|-|-|-|-|-|

- 初始化

	- Terminated initialization
```c++
char myword[] = { 'H', 'e', 'l', 'l', 'o', '\0' };	//该char类型数组包含6个数组元素，5个字符加上“\0"
```
	- String Initialization(Null-terminated)
	> `double qoutes ("")`自动追加空字符'\0'。

	```c++
char myword[] = "Hello"; 
	```
上面两种初始化方式得到的返回值都是含有6个元素的数组。

- String 与 空字符结尾的字符序列
> C++两种字符表示方式都支持

	- Example:
	```c++
// strings and NTCS:
#include <iostream>
#include <string>
using namespace std;

int main ()
{
  char question1[] = "What is your name? ";
  string question2 = "Where do you live? ";
  char answer1 [80];
  string answer2;
  cout << question1;
  cin >> answer1;
  cout << question2;
  cin >> answer2;
  cout << "Hello, " << answer1;
  cout << " from " << answer2 << "!\n";
  return 0;
}
	```
		- Return value
		```c++
What is your name? Homer
Where do you live? Greece
Hello, Homer from Greece!
		```

--------------------------
# Pointers
> **变量访问**：变量在计算机内存中的位置可以通过其`identifier`。
对于C ++程序，计算机的内存就像一系列内存单元（`memory cells`），每个内存单元大小为一个字节，每个内存单元具有唯一的地址。这些单字节内存单元的排序方式允许大于一个字节的数据表示占用具有连续地址的内存单元。

![](/images/c++/memaddspace.png)

- Address-of operator (&)
> <mark>pointer</mark>:存储其他变量地址的变量称为指针，指向其存储的地址所存储的变量。

获取变量地址的语法如下：
```c++
addr = &var	//变量var的地址被赋值给addr
```

- Dereference operator (*)
> Read \*: value pointed by. 
星号（\*）：asterisk.

解引用（获取指针所指向的变量值）语法如下：
```c++
baz = *ptr;	//baz equal to value pointed by ptr.该语句将指针ptr指向的变量的value赋值给baz。
```

- 指针操作
	- 指针声明
		- syntax
		```c++
type * name;	//"type"是指针指向数据/变量的类型
		```
		比如：
		```c++
int * number;	//number类型为 int*
char * character;	//character类型为 character*
double * decimals;	//decimals类型为 double*
		```
		上面例子中的三个指针因为存储的都只是所指数据的内存地址，所以指针本身占用的内存大小相同。但不同的操作系统可能相异。
		而星号(*)只是表示指针，与解引用是不同的解释。
		Example:
		```c++
// my first pointer
#include <iostream>
using namespace std;

int main ()
{
  int firstvalue, secondvalue;
  int * mypointer;

  mypointer = &firstvalue;
  *mypointer = 10;
  mypointer = &secondvalue;
  *mypointer = 20;
  cout << "firstvalue is " << firstvalue << '\n';
  cout << "secondvalue is " << secondvalue << '\n';
  return 0;
}
		```
		```c++
// more pointers
#include <iostream>
using namespace std;

int main ()
{
  int firstvalue = 5, secondvalue = 15;
  int * p1, * p2;

  p1 = &firstvalue;  // p1 = address of firstvalue
  p2 = &secondvalue; // p2 = address of secondvalue
  *p1 = 10;          // value pointed to by p1 = 10
  *p2 = *p1;         // value pointed to by p2 = value pointed to by p1
  p1 = p2;           // p1 = p2 (value of pointer is copied)
  *p1 = 20;          // value pointed to by p1 = 20
  
  cout << "firstvalue is " << firstvalue << '\n';
  cout << "secondvalue is " << secondvalue << '\n';
  return 0;
}
		```
			- Return value
			```c++
firstvalue is 10
secondvalue is 20
			```
		Note:
		`int * P1, * P2;`表示P1指针为int*类型，P2指针为int*类型;
		`int * P1, P2;`表示P1指针为int*类型，P2指针为int类型。
		
	- 指针与数组
	> 指针与数组标识符作用类似，但是指针可以被赋予新的地址，数组标识符（表示数组第一个元素的的地址）不能被赋予新的地址。

	```c++
int myarray [20];
int * mypointer;
mypointer = myarray;	//valid
myarray = mypointer;	//invalid
	```
	Example:
	```c++
// more pointers
#include <iostream>
using namespace std;

int main ()
{
  int numbers[5];
  int * p;
  p = numbers;  *p = 10;
  p++;  *p = 20;
  p = &numbers[2];  *p = 30;
  p = numbers + 3;  *p = 40;
  p = numbers;  *(p+4) = 50;
  for (int n=0; n<5; n++)
    cout << numbers[n] << ", ";
  return 0;
}
	```
		- Return value
		```c++
10, 20, 30, 40, 50, 
		```
	- 指针初始化
	> 指针在被声明时可以直接以内存地址初始化。比如：
	```c++
int myvar;
int * myptr = &myvar;	//结果等于int * myptr;myptr = &myvar;
	```

	- 指针算术运算
	> 指针支持算数运算（++，--，+1,-1）。

	- 常量指针
	> 常量指针仅读取所指向地址的value而不作修改。指针本身也可以是常量。

	```c++
int y=10;
const int * p = &y;

int x;
      int *       p1 = &x;  // non-const pointer to non-const int
const int *       p2 = &x;  // non-const pointer to const int
      int * const p3 = &x;  // const pointer to non-const int
const int * const p4 = &x;  // const pointer to const int 
	```

	- 字符串指针
	> 因为字符串是包含空白符字符序列的数组，故而，也可以使用指针访问string或存储string。

	```c++
const char * foo = "hello"; 
	```

	- 双层指针
		- Syntax
		```c++
type ** ptr_identifier;
		```
		Example:
		```c++
char a;
char * b;
char ** c;
a = 'z';
b = &a;
c = &b;
		```
		![](/images/c++/pointer_to_pointer.png)

	- 无类型指针
	> C++中`void`表示`absence of type`,或称无类型。所以`void *`类型的指针所指value没有类型（未定义长度、未定义解引用属性），
	因此`void *`类型指针可以指向任意类型的数据;而void一引入的缺点就是，不可以对`void *`指针直接使用`解引用 *`，
	所以无类型的地址需要在解引用前转化为某种特定类型的指针。
	
	```c++
// increaser
#include <iostream>
using namespace std;

void increase (void* data, int psize)
{
  if ( psize == sizeof(char) )
  { char* pchar; pchar=(char*)data; ++(*pchar); }
  else if (psize == sizeof(int) )
  { int* pint; pint=(int*)data; ++(*pint); }
}

int main ()
{
  char a = 'x';
  int b = 1602;
  increase (&a,sizeof(a));
  increase (&b,sizeof(b));
  cout << a << ", " << b << '\n';
  return 0;
}
	```
		- Return value
		```c++
y, 1603
		```

	- 无效指针与空指针
	> 原则上指针可指向任意地址，即使对应的地址未存入任何有效元素。比如未初始化指针与指向数组未定义元素位置的指针：

	```c++
int * p;               // uninitialized pointer (local variable)

int myarray[10];
int * q = myarray+20;  // element out of bounds 
	```
	<mark>**nullptr**</mark>:所有指针类型都可以指向的值，且所有空指针等同。C++会将`nullptr`解释为**an integer value of zero**或者**keyword: nullptr**.
	```c++
int * p = 0;
int * q = nullptr;
	```
		- void vs nullprt
		> 空指针是一个值，任何指针都可以表示它指向“nowhere”，而void指针是一种指针，可以指向某个没有特定类型的地方。一个是指存储在指针中的值，另一个是指它指向的数据类型。

	- 函数指针
	> 函数指针可以用于将函数作为参数传给另一个函数。
	函数指针与普通函数声明语义相同。

		- Syntax
		`type (*ptr_identifier)(types of func args) = func;`
		- Example
		```c++
// pointer to functions
#include <iostream>
using namespace std;

int addition (int a, int b)
{ return (a+b); }

int subtraction (int a, int b)
{ return (a-b); }

int operation (int x, int y, int (*functocall)(int,int))
{
  int g;
  g = (*functocall)(x,y);
  return (g);
}

int main ()
{
  int m,n;
  int (*minus)(int,int) = subtraction;

  m = operation (7, 5, addition);
  n = operation (20, m, minus);
  cout <<n;
  return 0;
}
		```
			- Return value
			```c++
			8
			```
----------------------
# Dynamic Memroy
> 在不同的程序中，存在这样一个需求：
程序的部分内存只有在`runtime`才能被确定。

C++语言包含两种对内存操作的函数：
1. new
2. delete

## operators new & new[]
`new`用于动态内存分配，语法实现上，其后跟着`type`;若需要申请内存以存放多个元素，则与要在最后加上`[the number of required elements]`。
- Syntax
```c++
pointer = new type;	//used to allocate memory containing one single element of type "type"
pointer = new type [number_of_elements];	//used to allocate a block (an array) of elements of type "type"
```

- Example
```c++
int * foo;
foo = new int [5];
```
	上述实例语句动态分配**5**个类型为**int**的元素并返回一个指向该五元素序列头部（第“0”个元素）的**pointer**，该指针指向一个包含五个**int**元素空间的内存块。
	- 访问动态内存
	**数组访问形式**： foo[0], foo[1], foo[2], ...
	**指针访问形式**： \*foo, \*[foo+1], \*[foo+2], ...
<mark>数组声明、动态内存申请的区别</mark>： 
> 编译时即需要确定数组的长度，即数组在声明时的长度表示必须是**constant expression**;
动态内存申请可以使用变量表示所需要申请的内存大小,在`runtime`时向`memory heap`动态申请内存空间。因为计算机内存有限，不保证动态申请操作的返回值，C++设置了异常机制，参见[Dynamic memory](http://www.cplusplus.com/doc/tutorial/dynamic/)。

## operators delete & delete[]
> C++使用`delete`释放被动态申请的内存空间以变回可用状态。

- Syntax
```c++
delete pointer;		//释放单个内存元素空间
delete[] pointer;	//释放内存块
```
> delete操作符有两种形式：
**pointer**、**null pointer**

- Example:
```c++
// rememb-o-matic
#include <iostream>
#include <new>
using namespace std;

int main ()
{
  int i,n;
  int * p;
  cout << "How many numbers would you like to type? ";
  cin >> i;
  p= new (nothrow) int[i];
  if (p == nullptr)
    cout << "Error: memory could not be allocated";
  else
  {
    for (n=0; n<i; n++)
    {
      cout << "Enter number: ";
      cin >> p[n];
    }
    cout << "You have entered: ";
    for (n=0; n<i; n++)
      cout << p[n] << ", ";
    delete[] p;
  }
  return 0;
}
```
	- Return value
	```c++
How many numbers would you like to type? 5
Enter number : 75
Enter number : 436
Enter number : 1067
Enter number : 8
Enter number : 32
You have entered: 75, 436, 1067, 8, 32,
	```

-------------------
# Data structure

## struct
> **struct**：一个数据结构使用同一个`identifier`表示一组`data elements`,这些数据元素被成为**members**成员，不同成员可以类型不同、长度不等。**struct**支持嵌套。

- Syntax
```c++
struct type_name {	//type_name: 该结构体的类型名称
member_type1 member_name1;
member_type2 member_name2;
member_type3 member_name3;
.
.
} object_names;		//object_name: 在该"type_name"类型的结构体的有效标识符子集内。
```
> 对象应当包含状态和行为，状态可以理解为成员，行为可以理解为成员函数。
--------------------
- Example
```c++
// array of structures
#include <iostream>
#include <string>
#include <sstream>
using namespace std;

struct movies_t {
  string title;
  int year;
} films [3];

void printmovie (movies_t movie);

int main ()
{
  string mystr;
  int n;

  for (n=0; n<3; n++)
  {
    cout << "Enter title: ";
    getline (cin,films[n].title);
    cout << "Enter year: ";
    getline (cin,mystr);
    stringstream(mystr) >> films[n].year;
  }

  cout << "\nYou have entered these movies:\n";
  for (n=0; n<3; n++)
    printmovie (films[n]);
  return 0;
}

void printmovie (movies_t movie)
{
  cout << movie.title;
  cout << " (" << movie.year << ")\n";
}
```
	- Return value
	```c++
Enter title: Blade Runner
Enter year: 1982
Enter title: The Matrix
Enter year: 1999
Enter title: Taxi Driver
Enter year: 1976
 
You have entered these movies:
Blade Runner (1982)
The Matrix (1999)
Taxi Driver (1976)
	```

### 结构指针

> 可使用结构类型名与指针标识符(*)定义指向结构的指针。

- Example
```c++
struct movies_t {
  string title;
  int year;
};

movies_t amovie;
movies_t * pmovie;
pmovie = &amovie;	//pmovie的value被复制为amovie的address
```

#### (->)解引用

> 指针使用解引用符号<mark>"->"</mark>访问具有**成员(member)**对象。

```c++
pmovie->title;
(*pmovie).title;
```
**指针访问对象成员 vs 对象访问自身成员**

|Expression	|What is evaluated	|Equivalent|
|-|-|-|
|a.b	|Member b of object a	| 	|
|a->b	|Member b of object pointed to by a	|(*a).b|
|*a.b	|Value pointed to by member b of object a|	*(a.b)|
|(*a).b| memebr of object pointed to by a | a->b|

## union
> 参见 [struct & union](https://leopold-sun.com/struct-unio.html)
unions允许内存块以不同的数据类型被访问到。

- Syntax
```c++
union type_name {
  member_type1 member_name1;
  member_type2 member_name2;
  member_type3 member_name3;
  .
  .
} object_names;
```
所有类型数据占用相同大小的内存空间，所占用的内存空间大小为所用类型中字节最多的类型所占用的空间大小。

## enum
> 参见 [enum](https://leopold-sun.com/enum.html)

- Syntax
```c++
enum type_name {
  value1,
  value2,
  value3,
  .
  .
} object_names;
```

## Type aliases
> typedef / using
C++允许使用`typede`和`using`为赋予类型`aliases`。

### typedef
> 为一般类型名赋予别名，继承自C语言。

- Syntax
```c++
typedef existing_type new_type_name ;
```

	- Example
	```c++
typedef char C;
typedef unsigned int WORD;
typedef char * pChar;
typedef char field [50]; 

C mychar, anotherchar, *ptc1;
WORD myword;
pChar ptc2;
field name; 
	```
### using
> c++语言引入的赋予类型别名的方式,比`typedef`的应用范围更广，主要是在`templates`的语法中。

- Syntax
```c++
using new_type_name = existing_type ;
```

	- Example
	```c++
using C = char;
using WORD = unsigned int;
using pChar = char *;
using field = char [50];
	```


------------------------------------

# Reference

- [C++ Array](http://www.cplusplus.com/doc/tutorial/arrays/)
- [Virtual address space in the context of programming](https://stackoverflow.com/questions/9511982/virtual-address-space-in-the-context-of-programming)
- [Reorganizing the address space](https://lwn.net/Articles/91829/)

