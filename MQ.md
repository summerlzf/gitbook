# MQ

MQ（Message Queue，消息队列）是一种消息中间件，主要用于组件之间的解耦，消息的发送者无需知道消息接收者的存在，反之亦然



# AMQP

AMQP（Advanced Message Queue Protocol，高级消息队列协议）是应用层协议上的一个开放标准，为面向消息的中间件设计

AMQP的特性：面向消息、队列、路由（包括：点对点、发布/订阅）、可靠性、安全性



# MQ应用场景

MQ的应用场景就是典型的生产者-消费者模式

- 当某一个时间段，生产者产生的消息频率（远）高于消费者消费消息的频率时，需要使用MQ，具体场景主要有：双11抢购，秒杀活动等
- 在消费者端需要一个接着一个处理，不允许插队，这是队列（Queue）的基本特性决定的
- 当需要进行消息分发（广播）的时候，也可以使用MQ



# RabbitMQ

RabbitMQ是一个开源的AMQP实现，服务端使用Erlang语言编写，支持多种客户端：Python、Ruby、.Net、Java、JMS、C、PHP、ActionScript、XMPP、STOMP等，支持Ajax，用于在分布式系统中的消息存储、转发，在易用性、扩展性、高可用性方面表现良好

### RabbitMQ的对象/组件

- ConnectionFactory：对外API中的基本对象，Connection对象的制造工厂
- Connection：同样是对外提供的API中的基本对象，封装了socket协议的相关逻辑
- Channel：对外API中的基本对象，是我们与RabbitMQ打交道的最重要的接口，大部分的业务逻辑都是在Channel接口中完成，包括定义Queue、定义Exchange、绑定Queue与Exchange、发布消息等
- Queue：RabbitMQ中的内部对象，存储队列消息本身
- Exchange：交换器/交换机，生产者将消息发送到Exchange，由Exchange根据路由规则将消息路由到一个或者多个Queue中，或者直接丢弃

### routing key

生产者将消息发送给Exchange的时候，一般会指定一个routing key，来确定消息的路由规则，这个routing key需要与Exchange Type及binding key联合使用才能最终生效，routing key的长度限制为255bytes

### binding key

RabbitMQ通过Binding（绑定）将Exchange与Queue关联起来，这样就能正确地将消息路由到指定的Queue，在Binding的同时，会指定一个binding key，当binding key与routing key匹配时，消息才会被路由到对应的Queue中

binding key并不会在所有情况下都生效，取决于Exchange Type，如fanout类型的Exchange Type会无视binding key，将消息路由到所有绑定该Exchange的Queue中

### Exchange Type

RabbitMQ中常用的Exchange Type有：fanout、direct、topic、headers

- fanout：路由规则非常简单，把所有发送到该Exchange的消息路由到所有与之绑定的Queue中
- direct：把消息路由到那些binding key和routing key完全匹配的Queue中
- topic：把消息路由到binding key和routing key基于规则匹配的Queue中，主要是模糊匹配，解决direct在实际需求中的局限，匹配规则为：用点号（.）分隔的字符串，两种特殊字符的匹配*（匹配一个任何单词）和#（匹配多个单词，可以是0个）
- headers：不依赖于routing key和binding key的匹配规则路由消息，而是根据发送的消息内容的headers属性进行匹配；在绑定Exchange和Queue时指定一组键值对，当Exchange接收到消息时，会获取消息的headers（也是键值对的形式）并进行匹配

### RPC

RabbitMQ本身是基于异步的消息处理，生产者发送消息后并不会等待、也不会知道消费者处理消息的结果，但在实际场景中，可能需要进行同步处理，即需要等待我发送的消息最终的处理结果，相当于RPC调用，RabbitMQ也支持RPC方式



# 选型和对比

|                   | Kafka | RabbitMQ | ActiveMQ | ZeroMQ |
| ----------------- | ----- | -------- | -------- | ------ |
| 社区活跃度        | 一般  | 高       | 一般     | 一般   |
| 消息持久化        |       | 支持     | 支持     | 不支持 |
| 综合技术实现【1】 | 好    | 好       | 一般     | 差     |
| 高并发            |       | 最好     |          |        |
| TPS               | 高    | 中       | 低       | 高     |

【1】综合技术实现，包括：可靠性、灵活的路由、集群、事务、高可用队列、消息排序、问题追踪、可视化管理工具、插件系统等



# Kafka与RabbitMQ的比较

1. RabbitMQ比Kafka成熟，在可用性、稳定性、可靠性上，RabbitMQ优于Kafka
2. Kafka设计的初衷是处理日志的，可以看作是一个日志系统，针对性强，因此不具备一个成熟的MQ应该具备的特性
3. Kafka的性能（吞吐量、TPS）比RabbitMQ要强很多，两者没有可比性