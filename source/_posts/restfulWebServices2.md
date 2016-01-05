title: restfulWebServices2
date: 2016-01-05 10:40:28
tags: [Web,Restful]
---

##Restful web Services ----第 2章 笔记
###识别资源
开发RESTful Web 服务的首要步骤之一就是设计资源模型，资源模型对所有客户端用来与服务器交互的资源加以识别和分类。

###如何选择资源粒度
通过网络效率、表达的多少以及客户端的易用程度来帮助确定资源的粒度。
粗粒度设计便于富客户端应用程序。而更精细的资源粒度可以更好地满足缓存的要求。因此，应从客户端和网络的角度确定资源的粒度。下列因素可能会进一步影响资源粒度：
- 可缓存性
- 修改频率
- 可变性
仔细设计资源粒度，以确保使用更多缓存，减小修改频率；或将不可变数据从使用缓存较少、修改频率更高或可变数据分离出来，这样可以改善客户端和服务端的效率。

###如何将资源组织为集合
将资源组织为集合，可以让客户端及服务器将一组资源视为一个资源来引用，在集合上执行查询，甚至将集合作为工厂来创建新资源。

基于应用程序特有条件来识别相似的资源，常见的例子有，共享同一数据库`Schema` 的资源、有相同特性`attribute`或属性`property`的资源或客户端看起来是相似的资源。

###如何支持计算或处理函数
将处理函数视为一个资源，使用HTTP GET来获取表述，其中包含处理函数输出。使用查询参数来为处理函数提供输入。REST只适用于应用程序领域中的“事物”或“实体”资源，这是对REST架构约束最常见的理解之一。

###何时及如何使用控制器来操作资源
就RESTful Web服务而言，控制器能帮助提升服务器与客户端的独立程度，改善网络效率，并让服务器以原子操作的形式来实现复杂操作。控制器是一种能以原子方式修改资源的资源，它能帮助服务器抽象复杂的业务操作，并为客户端提供触发这些操作的途径。

例子：
>客户端向要提供某书的30天免费在线版本，并带有15%折扣。服务器上维护了一个集合，其中包含所有正在提供30天免费访问的图书，客户端可以提交POST请求将该书添加到集合中：
```shell
#向三十天免费访问图书列表添加图书的请求
POST /30daybookoffers HTTP/1.1
Host: www.example.org
Content-Type:application/x-www-urlencoded
id=1234&from=2009-10-10&to=2000-11-10
#响应
HTTP/1.1 201 Created
Location: http://www.example.org/30dayebookoffer/1234
Content-length:0
```
如果业务上要求以原子凡是完成这两个修改，那么可以使用控制器资源：即添加折扣及30天免费访问的请求。

在请求中，参数` op=updateDiscount`和 `op=30dayOffer`用来标识操作，这样就会导致穿隧，穿隧会降低协议层面上的可见性，因为请求的可见部分（例如请求URI、使用HTTP方法、请求头和媒体类型）并不会明确地描述操作。要不惜一切代价避免穿隧，针对每个操作使用不同的资源。