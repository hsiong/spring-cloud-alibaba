

# 序言
重新写本文的意义在于, 深入学习微服务, 掌握"蓝绿、灰度、路由、限流、熔断、降级、隔离、追踪、流量染色、故障转移、多活"等概念和实现方式, 以及从原理上深入学习alibaba如何实现的这些功能

参考书籍目前主要有以下几个: 
+ [Spring Cloud Alibaba 笔记: 马士兵](./Spring_Cloud_Alibaba_笔记:马士兵.pdf)
+ [Spring Cloud 构建微服务架构: 翟永超](https://blog.didispace.com/spring-cloud-learning/)
+ [SpringBoot-Labs: 芋道源码(Ruoyi owner)](https://github.com/YunaiV/SpringBoot-Labs)

# 解决问题

![image](https://user-images.githubusercontent.com/37357447/148905888-9da0a5d0-9ce4-40ec-aadd-654f4807bc9f.png)
1. 应用拆分: 将一个单体应用按模块拆成多个应用, 实现**并发流量分担**和**功能水平拓展**

![image](https://user-images.githubusercontent.com/37357447/148906950-7bd61a3d-1a7c-4a47-891a-8f9f23e11835.png)
2. 将**重复的代码**抽取出来， 做成独⽴的服务对外暴露，前端控制层(表现层)通过服务注册中心调⽤服务(服务层)

![image](https://user-images.githubusercontent.com/37357447/148908164-33024b85-cf76-405e-93a7-0cc3f17e385c.png)
3. 每个服务都是一个可以独立运行的项目, **没有依赖关系**, 避免服务雪崩(上下游服务的崩溃, 导致整条服务链的崩溃)

# 怎么做
## 预备工作
1. 先建一个单体应用
2. 将**重复代码**按模块抽离, 做成单体服务

## 用 `RestTemplate` 开始改造
1. 通过 `RestTemplate` 进行项目间的通讯
2. 通过服务治理, 解决 `RestTemplate` 以下问题
+ 一旦服务提供者地址变化，就需要手工修改代码
+ 一旦是多个服务提供者，无法实现负载均衡功能
+ 一旦服务变得越来越多，人工维护调用关系困难

