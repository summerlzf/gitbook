# Dubbo与SpringCloud的区别

|              | Dubbo         | SpringCloud                 |
| ------------ | ------------- | --------------------------- |
| 服务注册中心 | Zookeeper     | SpringCloud Netflix Eureka  |
| 服务调用方式 | RPC           | HTTP（Rest API）            |
| 服务监控     | Dubbo-monitor | SpringBoot Admin            |
| 断路器       | 不完善        | SpringCloud Netflix Hystrix |
| 服务网关     | 无            | SpringCloud Netflix Zuul    |
| 分布式配置   | 无            | SpringCloud Config          |
| 服务跟踪     | 无            | SpringCloud Sleuth          |
| 消息总线     | 无            | SpringCloud Bus             |
| 数据流       | 无            | SpringCloud Stream          |
| 批量任务     | 无            | SpringCloud Task            |



## 主要区别

由于Dubbo使用基于RPC的调用方式，因此性能更好，但依赖性强，耦合度高；

而SpringCloud使用基于HTTP的REST调用方式，性能较差，但灵活性高，适合微服务的快速演进。



## 本质

Dubbo：一个提供服务治理的RPC框架

SpringCloud：微服务架构下的一整套解决方案



# @SpringBootApplication注解

除了四个元注解（@Target，@Retation，@Documented，@Inherited）外，还包括以下几个重要的注解：

#### @SpringBootConfiguration

继承自@Configuration，二者功能基本一致，标注当前类为Spring IOC容器的配置类，并将当前类内使用@Bean注解标记的方法实例纳入到IOC容器中，实例名就是方法名

#### @EnableAutoConfiguration

启动自动配置，借助@Import注解，将所有符合自动配置条件的bean定义加载到IOC容器

#### @ComponentScan

默认扫描当前包及其子包下所有符合条件的组件或者bean定义（@Service、@Component、@Repository、@Controller等），并将其加载到IOC容器中；可通过指定basePackages属性指定扫描的范围