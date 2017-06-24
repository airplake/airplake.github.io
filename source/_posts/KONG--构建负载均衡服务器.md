---
title: KONG--构建负载均衡服务器
date: 2017-06-24 16:45:38
categories: 后端开发
author: 老魏、陌上
tags:
  - KONG
  - 负载均衡
  - 高并发
---




## KONG--构建负载均衡服务器



### 什么负载均衡

先上图

![](./01.png)



首先我们先介绍一下什么是负载均衡: 负载平衡（Load balancing）是一种计算机网络技术，用来在多个计算机（计算机集群）资源中分配负载，以达到最佳化资源使用、最大化吞吐率、最小化响应时间、同时避免过载的目的。负载均衡的目的，就在于平衡负载，给用户提供优质，可靠，稳定的服务。当我们用户数据比较大的时候，单台服务器已经承受不了，这时候就需要服务器集群了，如何来分配服务器资源呢，这时负载均衡技术就出来。

<!-- more -->

### 什么是KONG

Kong是在客户端和（微）服务间转发API通信的API网关,提供丰富的插件。具体关于KONG的介绍请移步官网。


基本架构图
![基本架构图](./diagram-right.png)


> [kong  安装](https://getkong.org/install/)
> [kong  官网](https://getkong.org/docs/)
### 实例演示

今天我们主要的内容是如何使用KONG，搭建我们的负载均衡服务器，让你的网站也可以承受高并发的场景。下面就开始进行实例演示部分。



1. 创建Server
首先我们需要创建两个Server，它们所提供服务功能是一样的。

```javascript
  //  server 01
  //  app.js
 var express = require('express')
 var app = express()

  app.get('/', function (req, res) {                                                                                            
          res.send('server 01')  
  })

  app.listen(3000)
```

```javascript
  //  server 02
  //  app.js
 var express = require('express')
  var app = express()

  app.get('/', function (req, res) {                                                                                            
          res.send('server 02')  
  })

  app.listen(4000)
```

上面使用Express创建了两个非常简单的服务，访问根目录返回不同的字符(正常的服务返回的数据都是一样的，这里为了区分的确是访问了两个服务，所有返回不同的字符).

在浏览器中访问

[localhost:3000](localhost:3000)

[localhost:4000](localhost:4000)

得到正常的响应就可以接下面的步骤了。

> [express](https://github.com/expressjs/express)

2. 创建**Upstream**

```bash
curl -H "Content-Type: application/json" -X POST -d '{"name":"hello"}' http://127.0.0.1:8001/upstreams/
```
你也可以使用**POSTMAN**，或者其它API工具。

>  [add-upstream 参考](https://getkong.org/docs/0.10.x/admin-api/#add-upstream)

3. 创建**Target**

分别执行

```bash
curl -H "Content-Type: application/json" -X POST -d '{"target": "127.0.0.1:3000", "weight":  100 }' http://127.0.0.1:8001/upstreams/hello/targets
```

```bash
curl -H "Content-Type: application/json" -X POST -d '{"target": "127.0.0.1:4000", "weight":  100 }' http://127.0.0.1:8001/upstreams/hello/targets
```

你也可以使用**POSTMAN**，或者其它API工具。

> [add-target 参考](https://getkong.org/docs/0.10.x/admin-api/#add-target)

4. 创建**Api**  

```bash
curl -H "Content-Type: application/json" -X POST -d '{"name":"hello","hosts":"hello.com","upstream_url":"http://hello","strip_uri":false}' http://127.0.0.1:8001/apis/
```

你也可以使用**POSTMAN**，或者其它API工具。

> [add-api 参考](https://getkong.org/docs/0.10.x/admin-api/#add-api)

5. 测试

然后使用POSTMAN去访问，会发现不一样的东西，需要多刷新几次。

效果如果

![](./result-pre.png)


6. 注意

* 特别需要注意是你的IP,如果你KONG是装在本机，可以用localhost或者127.0.0.1,如果是装在其它机器，ip应对那台机器
* 这是说的负载均衡，主要指的web服务器的负载均衡。


### 结论

负载均衡技术是一项非常重要的技术，可以很好的应对高并发的场景，最大化的利用服务器资源，希望大家可以掌握、学会。



### 广告时间

[Airplake](http://airplake.com/about/)  是由远程全职，兼职工程师组成的合作社区，社区采用会员制。社区中大部分会员就职于各互联网技术公司，也有全职独立工作者。为提高远程协作的工作效率和流畅性，保证会员兼职收益，在Airplake中的会员需要较为严格的遵循一些项目合作的规则，以保证项目进度和质量。

![](airplake.png)
