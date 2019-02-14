---
title: Introduction to Envoy
date: 2018-12-27 13:03:06
tags: [Envoy]
categories: [Envoy]
description:
keywords: envoy
---

<center>*The network should be transparent to applications. When network and application problems do occur it should be easy to determine the source of the problem.* </center>
>> [Envoy](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy)

<!-- more  -->

# Envoy 

## What is Envoy?

> Envoy is an L7 proxy and communication bus designed for large modern service oriented architectures.

### Related Concepts

> [gRPC](https://grpc.io/docs/)
> [Envoy gRPC](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/grpc)
> [data plane api](https://github.com/envoyproxy/data-plane-api/blob/master/API_OVERVIEW.md)

## High-level features provided by Envoy

- Out of process architecture (非入侵式架构)
> `Envoy`是和应用服务并行运行的，透明地代理应用服务发出/接收的流量。应用服务只需要和`Envoy`通信，无需知道其他微服务应用在哪里。
- Modern C++11 code base
- L3/L4 filter architecture (3/4层过滤器)
> L3、L4代理是`Envoy`的核心，使用`pluggable filter chain mechanism`插件式过滤系执行TCP、UDP任务，如TCP转发、TLS认证。
- HTTP L7 filter architecture (7层过滤器)
> `Envoy`内置`http_connection_manager`（可以通过系列化http过滤器执行http协议层面的任务）。
- First class HTTP/2 support （支持http/2）
- HTTP L7 routing
- gRPC support
- MongoDB L7 support
- DynamoDB L7 support
- Service discovery and dynamic configuration
- Health checking
- Advanced load balancing
- Front/edge proxy support
- Best in class observability

### Network (L3/L4) Filters

The L3/L4 filters are the core of Envoy connection handling, which is comprised of three types of network filters:
- Read: Envoy invokes `Read Filter` when receiving data from a downstream host connection.
- Write: Envoy invokes `Write Filter` when sending  data to a downstream host connection.
- Read/Write: Envoy invokes `Read/Write Filter` when receiving and sending data from/to a downstream host connection at a same time.

## Teminology

`Host`: 有能力进行网络通信的逻辑网络应用。
`Downstream`: 下游主机建立与Envoy之间的连接并发送请求、接收响应。
`Upstream`: 上游主机接收Envoy发来的连接与请求并作出对请求的响应。
`Listener`: 是Envoy监听的一个地址,供下游主机连接的“已命名”的网络位置（类似与端口、Unix domain socket）,Envoy会暴露一个或多个监听器以供下游主机连接。
`Cluster`: Envoy所连接的一组逻辑功能类似的上游主机.Envoy通过[service discovery](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/service_discovery#arch-overview-service-discovery)发现集群成员;通过[active health checking](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/health_checking#arch-overview-health-checking)决定集群成员的健康状态;Envoy通过[load balancing policy](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/load_balancing/overview#arch-overview-load-balancing)据定将request route给那一个集群成员。
`Mesh`:  一群互相协作的主机共同组成的网络拓扑关系。`Envoy Mesh`表示一组Envoy代理，这组代理为一个分布式系统（包含不同服务与应用）形成一个`messgae passing substrate`信息传输层。
`Runtime configuration`: Envoy的配置子系统.修改envoy配置且不需要重启envoy或修改前（primary）配置以使之生效。
`Http Route Table`: HTTP 的路由规则，例如请求的域名，Path符合什么规则，转发给哪个 Cluster。

## Threading Model —— SPMT Model

> Envoy 采用 <mark>单进程多线程架构</mark>.。主线程负责控制稀疏的协作任务，同时其他的一些线程负责**监听**、**过滤**和**转发**的任务。 每个监听器（也是一个worker thread）负责对应监听到的整个连接的生命周期。这也就导致了大多数Envoy都是单线程的，且带有少量负责与其他工作线程通讯的复杂代码。Envoy是[非阻塞式的](https://www.zhihu.com/question/19732473)。——[Envoy Threading Model](https://www.envoyproxy.io/docs/envoy/latest/intro/arch_overview/threading_model)

## Listeners

- 当前Envoy仅支持TCP Listeners
- 推荐每台机器运行一个Envoy进程以简化操作和实现单源数据

# Envoy as a Front-Proxy

## Architecture

- ![](/images/envoy/front-envoy-archi.jpg)


-------------------------------

# References

- [yandy在掘金](https://juejin.im/post/5ad6fb06518825364001f619)
- [Envoy Configuration Reference](https://www.envoyproxy.io/docs/envoy/latest/configuration/configuration)
- [Getting started with Lyft Envoy for microservices resilience](https://www.datawire.io/envoyproxy/getting-started-lyft-envoy-microservices-resilience/)

