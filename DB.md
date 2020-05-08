# 数据库连接池对比

参与对比的主流数据库连接池包括：hikari，druid，c3p0，dbcp，tomcat-jdbc

#### 基于MySQL的测试结论

- 性能：hikari > druid > tomcat-jdbc > dbcp > c3p0，hikari的高性能得益于最大限度地避免锁竞争
- druid功能最为全面，sql拦截等功能，统计数据较为全面，具有良好的扩展性
- 开启prepareStatement cache，性能大概有20%的提升
- 综合性能、扩展性等方面，优先考虑使用 druid 和 hikari 连接池

#### 功能对比

| 性能由高到低     | hikari                              | druid              | tomcat-jdbc        | dbcp                 | c3p0                               |
| ---------------- | ----------------------------------- | ------------------ | ------------------ | -------------------- | ---------------------------------- |
| 是否支持PS cache | 否                                  | 是                 | 否                 | 是                   | 是                                 |
| 监控             | jmx                                 | jmx, log, http     | jmx                | jmx                  | jmx, log                           |
| 扩展性           | 弱                                  | 强                 | 弱                 | 弱                   | 弱                                 |
| sql拦截及解释    | 无                                  | 支持               | 无                 | 无                   | 无                                 |
| 代码复杂度       | 简单                                | 中等               | 简单               | 简单                 | 复杂                               |
| 连接池管理       | ThreadLocal + CopyOnWrite ArrayList | 数组               | FairBlocking Queue | LinkedBlocking Queue |                                    |
| 特点             | 优化力度大，功能简单，起源于boneCP  | 阿里开源，功能全面 |                    | 依赖于common-pool    | 历史久远，代码逻辑复杂，且不易维护 |


