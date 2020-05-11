# 数据库连接池对比

参与对比的主流数据库连接池包括：hikariCP，druid，c3p0，dbcp，tomcat-jdbc

#### 基于MySQL的测试结论

- 性能：hikariCP > druid > tomcat-jdbc > dbcp > c3p0，hikari的高性能得益于最大限度地避免锁竞争
- druid功能最为全面，sql拦截等功能，统计数据较为全面，具有良好的扩展性
- 开启prepareStatement cache，性能大概有20%的提升
- 综合性能、扩展性等方面，优先考虑使用 druid 和 hikariCP 连接池

#### 功能对比

| 性能由高到低     | hikariCP                            | druid              | tomcat-jdbc        | dbcp                 | c3p0                               |
| ---------------- | ----------------------------------- | ------------------ | ------------------ | -------------------- | ---------------------------------- |
| 是否支持PS cache | 否                                  | 是                 | 否                 | 是                   | 是                                 |
| 监控             | jmx                                 | jmx, log, http     | jmx                | jmx                  | jmx, log                           |
| 扩展性           | 弱                                  | 强                 | 弱                 | 弱                   | 弱                                 |
| sql拦截及解释    | 无                                  | 支持               | 无                 | 无                   | 无                                 |
| 代码复杂度       | 简单                                | 中等               | 简单               | 简单                 | 复杂                               |
| 连接池管理       | ThreadLocal + CopyOnWrite ArrayList | 数组               | FairBlocking Queue | LinkedBlocking Queue |                                    |
| 特点             | 优化力度大，功能简单，起源于boneCP  | 阿里开源，功能全面 |                    | 依赖于common-pool    | 历史久远，代码逻辑复杂，且不易维护 |

#### 性能测试结论

- hikariCP，号称Java平台最快的数据库连接池
- hikariCP 在高并发的情况下，性能基本上没有下降
- c3p0 性能最差，随着并发的提高，性能急剧下降，不建议使用

#### hikariCP为什么这么快

- 使用更好的并发集合类ConcurrentBag（专门为连接池设计的lock-less集合），实现了比LinkedBlockingQueue、LinkedTransferQueue更好的并发性能
- Statement集合存储使用FastList代替ArrayList，减少不必要的rangeCheck，以及进行其他优化
- 使用ThreadLocal缓存链接，大量使用CAS，最大限度避免lock
- 从字节码维度进行优化，使用第三方的Java字节码修改类库Javassist委托生成实现动态代理，相比JDK Proxy生成的字节码更少（精简了很多不必要的字节码），速度更快