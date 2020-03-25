# `Spring Cloud`

- 服务治理  Eureka
- 服务通信  Ribbon
- 服务通信  Feign
- 服务网关  Zuul
- 服务跟踪  Zipkin
- 服务监控  Actuator
- 服务配置  Config
- 服务容错  Hystrix

---

服务治理的核心分为3个部门：  服务提供者， 服务消费者， 注册中心 

Spring Cloud的服务治理使用Eureka来实现， Eureka是Netfix开源的基于REST的服务治理解决方案，Spring Cloud集成了Eureka，提供服务注册和服务发现的功能。

## `Spring Cloud Eureka`

- Eureka Server， 注册中心
- Eureka Client， 所有要进行注册的微服务通过 Eureka Client 连接到Eureka Server， 完成注册

## `RestTemplate的使用`

- 什么是RestTemplate？

  RestTemplate是Spring框架提供的基于Rest的服务组件，底层是对HTTP请求及响应进行了封装，提供了很多访问REST服务的方法，可以简化代码的开发。

