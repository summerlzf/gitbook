# 缓存

使用缓存是能够大幅提升性能的最重要方法，市面上常见的缓存中间件有：Memcached，Redis

### 缓存穿透

指查询一个数据库必然不存在的数据。

正常使用缓存的流程大致是：先进行缓存查询，如果缓存不存在或者已经过期，就对数据库进行查询，并将查询到的结果写入缓存，如果数据库查询不到（查询为空），则不写入缓存，如下代码示例

```java
/**
 * 获取用户信息（缓存的正常使用流程）
 */
public User getUser(String id) {
    // 查询redis缓存
    User obj = (User)redisTemplate.opsForValue().get(id);
    if (obj != null) {
        return obj;
    }
    // 查询数据库
    User user = userDao.getById(id);
    if (user != null) {
        // 写入缓存，保存60分钟
        redisTemplate.opsForValue().set(id, user, 60, TimeUnit.MINUTES);
    }
    return user;
}
```

如果此时传递一个数据库必然不存在的用户id（如：“0”），那么每次查询必然落到数据库上，因为缓存中也一定不存在对应key（用户id）的值，这就是<u>缓存穿透</u>，恶意攻击者可以利用这个漏洞发起密集请求，进而压垮数据库。

如何解决这个问题，业内较为成熟的解决方案有两种：

- 布隆过滤器（Bloom Filter）：类似于哈希表的算法，将所有可能的查询条件生成一个bitmap，在进行数据库查询之前使用这个bitmap进行过滤，从而减轻数据库层面的压力
- 空值缓存：也就是即便数据库查询不出来（查询出来的是空值），我们同样写入到缓存，只是缓存的超时时间可以设置相对较短，具体可见如下修改后的代码

```java
/**
 * 获取用户信息（防止缓存穿透）
 */
public User getUser(String id) {
    // 查询redis缓存
    User obj = (User)redisTemplate.opsForValue().get(id);
    if (obj != null) {
        // 判断是否为空的User对象，如果是，直接返回null，否则返回该对象
        return obj.getId() == null ? null : obj;
    }
    // 查询数据库
    User user = userDao.getById(id);
    if (user != null) {
        // 写入缓存，保存60分钟
        redisTemplate.opsForValue().set(id, user, 60, TimeUnit.MINUTES);
    } else {
        // 创建一个空的User对象写入缓存
        redisTemplate.opsForValue().set(id, new User(), 1, TimeUnit.MINUTES);
    }
    return user;
}
```

### 缓存雪崩

指在某一个时间段，缓存集中过期失效。

针对某些数据集中创建的缓存，可能会设置相同的周期时间，缓存集中失效后，数据库会在集中失效的时间段内承受直接请求的访问压力，对数据库而言，这种压力是周期性的。针对这种情况，解决办法是设置不同的过期时间，一般是加上一个随机因子，从而分散其超时时间

### 缓存击穿

指一个key非常热点，大量集中的并发请求都落到这一个点上，当这个key在失效的瞬间，大量高并发请求穿过缓存，直接抵达数据库，对数据库构成瞬间的压力

解决方法：使用互斥锁实现对数据库的单例访问，具体可见如下的代码实现

```java
static Lock lock = new ReentrantLock();

public List<String> getData() {
    // 从缓存中读取数据
    List<String> result = getDataFromCache();
    if (result.isEmpty()) {
        // 尝试获取锁
        if (lock.tryLock()) {
            try {
                // 从数据库获取数据
                result = getDataFromDB();
                // 写入缓存
                setDataToCache(result);
            } finally {
                // 释放锁
                lock.unlock();
            }
        } else {
            // 没有获取到锁，再查一下缓存
            result = getDataFromCache();
            if(result.isEmpty()) {
                Thread.sleep(100);
                // 重试
                return getData();
            }
        }
    }
    return result;
}
```