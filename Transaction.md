# 事务的四个特性（ACID）

#### 原子性（Atomicity）

要么全部执行成功，要么全部不执行，只要其中一条指令失败，所有执行失败，数据进行回滚，回到执行前的状态

#### 一致性（Consistency）

事务的执行使数据从一种状态转换为另一种状态，但是对于整个数据的完整性保持稳定

#### 隔离性（Isolation）

当多个用户并发访问数据库，如访问同一张表时，数据库为每个用户开启的事务，不能被其它事务的操作所干扰，多个并发事务之间要相互隔离

#### 持久性（Durability）

当事务执行完成后，它对于数据的改变是永久性的



# 并发事务导致的问题

#### 丢失更新

- 情况一：撤销一个事务时，把其他事务已提交的更新数据覆盖
- 情况二：不可重复读的特殊情况，两个事务同时读取了同一条数据，然后都进行了写操作，并提交，第一个事务所作的改变就会丢失

#### 脏读

在一个事务处理过程中读取了另一个未提交的事务中的数据

#### 幻读（又叫：虚读）

一个事务执行两次查询，第二次的结果与第一次结果不一致（出现没有或者已经被删除的行数据），是因为另一个事务在两次查询之间插入或者删除了数据造成的。幻读是事务非独立执行时发生的一种现象

#### 不可重复读

一个事务两次读取了同一行数据，结果得到不同状态的数据，中间正好另一个事务更新了该数据，两次结果相异



### 不可重复读 与 脏读 的区别

脏读是某一事务读取了另一个事务未提交的数据（脏数据），而不可重复读则是读取另一个事务已经提交了的数据

### 不可重复读 与 幻读 的区别

两者都是读取了另一个事务已经提交了的数据，不同的是，不可重复读查询的都是同一个数据项（同一条记录），而幻读针对的是一批数据整体（如数据的个数）



# 数据库事务隔离级别

有4种事务隔离级别，由低到高分别：

- Read uncommitted（读未提交）：最低的隔离级别，任何情况都无法保证，就是一个事务可以读取另一个事务未提交的数据
- Read committed（读提交）：可以避免脏读的发生，就是一个事务要等到另一个事务提交后才能读取数据
- Repeatable read（重复读）：可以避免脏读、不可重复读的发生，就是开始读取数据（事务开启）时，不再允许修改（update）操作；**注意**，这里是不允许update，但是允许insert，所以还是会有可能出现幻读问题
- Serializable（序列化）：可以避免脏读、不可重复读、幻读的发生，最高的隔离级别，就是所有事务串行化顺序执行，但是效率低下，消耗数据库性能，一般不采用

### 主流数据库的默认事务隔离级别

- Read committed：SQL Server，Oracle
- Repeatable read：MySQL