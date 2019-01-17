---
title: front proxy route policy
tags: [Envoy]
categories: [Envoy]
date: 2019-01-14 13:27:58
---

# KubeCon Abstract

Front Proxy is a representative sandbox of Envoy service mesh. The routing policy of Envoy proxy will inevitably limit the forwarding rate of packets when the number of services it serves grows to be large such as 200. Guoao Sun will review the common routing policy of envoy proxy, discuss one crucial factor interfering its performance, and propose one optimization method to address this challenge.

<!-- more -->
-------------

# Preliminaries

> [2016: A Comprehensive Guide To Singly Linked List Using C++](https://www.codementor.io/codementorteam/a-comprehensive-guide-to-implementation-of-singly-linked-list-using-c_plus_plus-ondlm5azr)

## C/C++ 链表

> A linked list is a data structure that can store an indefinite amount of items. These items are connected using pointers in a sequential manner.

### 数组与链表优劣比较

| comparison | array | linklist |
| --- | --- | --- |
| address | 分配连续的内存地址 | 用时分配，内存地址可以不连续 |
| search | O(1) | O(n) |
| add/delete | O(n) | O(1) |
| size | 数组在被定义时大小被固定 | 按需动态地申请内存 |
| data type | 同一个数组元素类型一致 | 链表结构可存储多种类型的数据 |
| type | array | 单链表，循环链表，双向链表 |

### 链表

### 链表类别

1. 单链表<singly-linked list>
> 每个链表元素包含存储的数值和指向后驱元素的link，最后一个元素指向NULL。
2. 双向链表<doubly-linked list>
> 双向链表的元素包含存储的数值与前、后驱link。

### 链表元素

- 一般元素
> linklist的元素被定义为`node`。`node`结构包含`data`和`next`两部分。`data`部分可以包含不止一个数值变量，`next`存储`the next node`的地址，也即是表示上述
- 头部元素
> 链表的第一`node`被称为`head`。
- 尾部元素
> 链表的最后一个`node`被称为`tail`。

### 链表的C++实现

```c++
struct node
{
  int data;
  node *Next;
};
```

#### C++链表创建

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
    
    //creation
    void creatnode(int value)
    {
      node *temp=new node;
      temp->data=value;
      //最后一个node的next指向NULL
      temp->next=NULL;
      if(head==NULL)
      {
        head=temp;
        tail=temp;
        //已将temp地址赋值给head和tail，temp的值（其表示的地址）已经不需要了
        temp=NULL;
      }
      else
      {
        tail->next=temp;
        tail=temp;
      }
    }
};
```

------------------------

# Route Policy Analysis

## Locate router.h

### Used commands

```bash
$ find . -name *route*
./docs/root/api-v2/http_routes
./docs/root/api-v2/http_routes/http_routes.rst
./docs/root/install/tools/route_table_check_tool.rst
./docs/root/configuration/thrift_filters/router_filter.rst
./docs/root/configuration/http_conn_man/route_matching.rst
./docs/root/configuration/tools/router_check.rst
./docs/root/configuration/http_filters/router_filter.rst
./api/envoy/config/filter/thrift/router
./api/envoy/config/filter/thrift/router/v2alpha1/router.proto
./api/envoy/config/filter/http/router
./api/envoy/config/filter/http/router/v2/router.proto
./api/envoy/config/filter/network/thrift_proxy/v2alpha1/route.proto
./api/envoy/api/v2/route
./api/envoy/api/v2/route/route.proto
./test/extensions/filters/http/router
./test/extensions/filters/network/thrift_proxy/router_test.cc
./test/extensions/filters/network/thrift_proxy/router_ratelimit_test.cc
./test/extensions/filters/network/thrift_proxy/route_matcher_test.cc
./test/tools/router_check
./test/tools/router_check/router.h
./test/tools/router_check/router_check.cc
./test/tools/router_check/router.cc
./test/tools/router_check/test/route_tests.sh
./test/mocks/router
./test/common/router
./test/common/router/route_fuzz.proto
./test/common/router/router_test.cc
./test/common/router/router_ratelimit_test.cc
./test/common/router/router_upstream_log_test.cc
./test/common/router/route_fuzz_test.cc
./test/common/router/route_corpus
./test/common/router/route_corpus/clusterfuzz-testcase-minimized-route_fuzz_test-5654717359718400
./test/common/router/route_corpus/clusterfuzz-testcase-minimized-route_fuzz_test-5142800207708160
./test/common/router/route_corpus/clusterfuzz-testcase-minimized-route_fuzz_test-5198208916520960
./test/common/router/route_corpus/clusterfuzz-testcase-route_fuzz_test-5647162250625024
./test/common/json/config_schemas_test_data/test_route_configuration_schema.py
./test/common/json/config_schemas_test_data/test_route_entry_schema.py
./test/common/json/config_schemas_test_data/test_http_router_schema.py
./configs/envoy_router_v2.template.yaml
./include/envoy/router
./include/envoy/router/router_ratelimit.h
./include/envoy/router/router.h
<mark>./include/envoy/router/route_config_provider_manager.h
./source/extensions/filters/http/router
./source/extensions/filters/network/thrift_proxy/router
./source/extensions/filters/network/thrift_proxy/router/router_ratelimit.h
./source/extensions/filters/network/thrift_proxy/router/router.h
./source/extensions/filters/network/thrift_proxy/router/router_ratelimit_impl.cc
./source/extensions/filters/network/thrift_proxy/router/router_impl.cc
./source/extensions/filters/network/thrift_proxy/router/router_ratelimit_impl.h
./source/extensions/filters/network/thrift_proxy/router/router_impl.h
./source/common/router
./source/common/router/router_ratelimit.h
./source/common/router/router.h
./source/common/router/router.cc
./source/common/router/router_ratelimit.cc</mark>
```

Then we mark the relative libraries and source files.从bash的返回值可知，`Envoy Router`的代码分为三个主要部分：
1. router.h/cc
2. router_ratelimit.h/cc
3. route_config_provider_manager.h

### Logic

`Route Table`在我们部署Envoy Proxy时应该是自动化部署的。根据我对Front-Proxy的观察和沙箱测试，扩展services的数量时并不需要对`Envoy Router`做改动，而是实现自动化部署的效果。Front-Proxy`Route config`的配置细节如下：

Location: `~/envoy/examples/front-proxy`
Partial content:
```bash
    filter_chains:
    - filters:
      - name: envoy.http_connection_manager
        config:
          codec_type: auto
          stat_prefix: ingress_http
          route_config:
            name: local_route
            virtual_hosts:
            - name: backend
              domains:
              - "*"
              routes:
              - match:
                  prefix: "/service/1"
                route:
                  cluster: service1
              - match:
                  prefix: "/service/2"
                route:
                  cluster: service2
          http_filters:
          - name: envoy.router
            config: {}
```

这意味着，当Envoy Proxy的配置文件完成后，`Route Table`的runtime行为就相对落实了。接下来则是查阅`Envoy Router`的实现以分析其`Router Policy`。

### router.h 与 router.cc
