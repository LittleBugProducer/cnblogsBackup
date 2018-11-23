
*说明：用时 from 2018-11-16 to 2018-11-23 七天*
###0 放在前面
&emsp;&emsp;什么是微服务？

&emsp;&emsp;**微服务是一个分布式系统。**微服务架构的风格，就是将单一程序开发成一个微服务，每个微服务运行在自己的进程中，并使用轻量级机制通信，通常是HTTP RESTFUL API。这些服务围绕业务能力来构建划分，并通过完全自动化部署机制来独立部署。这些服务可以使用不同的编程语言，以及不同数据存储技术，以保证最低限度的集中式管理。

&emsp;&emsp;Spring Cloud依赖于Spring Boot，可以快速开发，持续交付。Spring Boot简化了基于Spring的开发，大大提高了开发效率。
###1 模块简介
&emsp;&emsp;依书中序顺，Spring Cloud中模块依次为：eureka->ribbon->feign->hystrix->zuul->config->sleuth->admin->security->oauth

eureka：类似于dubbo的zookeeper。通过server和client，完成不同服务之间的调用。

ribbon：负载均衡，降低单个节点的压力

feign：基本功能同eureka

hystrix：熔断器。降低单个节点异常无法服务对整个系统的影响，自动隔离该节点。

zuul：路由网关，统一暴露api，保护系统，权限管理，统一监控

config：系统配置。管理系统各项配置文件，实现自动配置更新

security、oauth：系统安全，方法级保护系统。
###2 学习总结
&emsp;&emsp;SpringCloud在应用中，在application.yml中即可进行多项配置，减少了很多配置。许多配置也有其默认值，不影响使用。

&emsp;&emsp;本书中每个模块自成一章，方便学习。缺乏实战指导，需要在实际项目工作中多加思考，如何更好的集成应用SpringCloud提高系统可用性。所有服务均注册于eureka，eureka server本身支持集中部署，因此，可以适用于大型网站应用。

&emsp;&emsp;书末案例可作为SpringCloud为主的应用开发基本架构，但缺乏与其他框架的综合使用，所以未动手实践。作为低级程序员技术积累，扩大技术面，本书推荐学习。
