---
title: linklist
tags: [CPP]
categories: [CPP,C++收录]
date: 2019-01-17 16:49:15
description:
---

# C/C++ 链表

> A linked list is a data structure that can store an indefinite amount of items. These items are connected using pointers in a sequential manner.

## 数组与链表优劣比较

| comparison | array | linklist |
| --- | --- | --- |
| address | 分配连续的内存地址 | 用时分配，内存地址可以不连续 |
| search | O(1) | O(n) |
| add/delete | O(n) | O(1) |
| size | 数组在被定义时大小被固定 | 按需动态地申请内存 |
| data type | 同一个数组元素类型一致 | 链表结构可存储多种类型的数据 |
| type | array | 单链表，循环链表，双向链表 |

<!-- more -->

## 链表

## 链表类别

1. 单链表<singly-linked list>
> 每个链表元素包含存储的数值和指向后驱元素的link，最后一个元素指向NULL。
2. 双向链表<doubly-linked list>
> 双向链表的元素包含存储的数值与前、后驱link。

## 链表元素

- 一般元素
> linklist的元素被定义为`node`。`node`结构包含`data`和`next`两部分。`data`部分可以包含不止一个数值变量，`next`存储`the next node`的地址，也即是表示上述
- 头部元素
> 链表的第一`node`被称为`head`。
- 尾部元素
> 链表的最后一个`node`被称为`tail`。

## 链表的C++实现

```c++
struct node
{
  int data;
  node *Next;
};
```

### C++链表创建、打印、插入、删除

```c++
class list
{
  Private:
    node *head,*tail;
  Public:
    list()
    {
      //指向NULL避免产生内存垃圾
      head=NULL;
      tail=NULL;
    }
    
    //链表创建
    void creatnode(int value)
    {
      node *temp=new node;
      temp->data=value;
      //最后一个node的next指向NULL
      temp->next=NULL;
      //因为链表head指向第一个节点，所以若是“head==NULL”，则表示链表为空
      if(head==NULL)
      {
        head=temp;
        //若是链表只有一个节点，则head与tail指向同一个节点
        tail=temp;
        //已将temp地址赋值给head和tail，temp的值（其表示的地址）已经不需要了
        temp=NULL;
      }
      else
      {
        //新节点地址传递给tail节点
        tail->next=temp;
        tail=temp;
      }
    }

    //链表打印
    void displdy()
    {
      node *temp=new node;
      temp=head;
      while(temp!=NULL)
      {
        cout<<temp->data<<"\t";
        temp=temp->next;
      }
    }

    //链表插入
    /*需要考虑三种插入结果：
    *头部插入
    *尾部插入
    *特定位置插入
    */
    void insert_start(int value)
    {
      node *temp=new node;
      temp->data=value;
      temp->next=head;
      head=temp;
    }

    void insert_end(int value)
    {
      node *temp=new node;
      temp->data=value;
      temp->next=NULL;
      tail->next=temp;
      tail=temp;
    }

    void insert_position(int pos,int value)
    {
      node *pre=new node;
      node *cur=new node;
      node *temp=new node;
      temp->data=value;
      cur=head;
      for(int i=1;i<pos;i++)
      {
        pre=cur;
        cur=pre->next;
      }
      pre->next=temp;
      temp->next=cur;
    }
     
    /*链表删除
    *删除链表头部
    *删除链表尾部
    *删除链表特定位置
    */
    void delete_first()
    {
      node *temp=new node;
      temp=head;
      head=head->next;
      delete temp;
    }

    void delete_last()
    {
      node *current=new node;
      node *previous=new node;
      current=head;
      while(current->next!=NULL)
      {
        previous=current;
        current=current->next;	
      }
      tail=previous;
      previous->next=NULL;
      delete current;
    }

    void delete_position(int pos)
    {
      node *current=new node;
      node *previous=new node;
      current=head;
      for(int i=1;i<pos;i++)
      {
        previous=current;
        current=current->next;
      }
      previous->next=current->next;
    }
};
```
## Example

### Source Code

> From [kamal-choudhary](https://github.com/kamal-choudhary/singly-linked-list/blob/master/Linked%20List.cpp)

### Compile and execution

```bash
$ g++ linklist.cpp -o linklist
$ ./linklist

--------------------------------------------------
---------------Displaying All nodes---------------
--------------------------------------------------
25	50	90	40	
--------------------------------------------------
-----------------Inserting At End-----------------
--------------------------------------------------
25	50	90	40	55	
--------------------------------------------------
----------------Inserting At Start----------------
--------------------------------------------------
50	25	50	90	40	55	
--------------------------------------------------
-------------Inserting At Particular--------------
--------------------------------------------------
50	25	50	90	60	40	55	
--------------------------------------------------
----------------Deleting At Start-----------------
--------------------------------------------------
25	50	90	60	40	55	
--------------------------------------------------
-----------------Deleing At End-------------------
--------------------------------------------------
25	50	90	60	40	
--------------------------------------------------
--------------Deleting At Particular--------------
--------------------------------------------------
25	50	90	40	
--------------------------------------------------
```


----------------

# Reference

- [linklist example](https://github.com/kamal-choudhary/singly-linked-list/blob/master/Linked%20List.cpp)
