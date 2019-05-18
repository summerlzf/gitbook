# Java类加载机制：双亲委派模型

#### 由上到下的类加载器：

启动类加载器（Bootstrap ClassLoader）
扩展类加载器（Extension ClassLoader）
应用类加载器（Application ClassLoader）
自定义类加载器

双亲委派模型的本质：确保所在层级的类被正确加载（最终实现的统一）



# Java内存模型（JMM）

【待续】



# 垃圾回收（GC）机制

【待续】



# Java动态代理和cglib实现

JDK动态代理：通过接口中的方法名，在动态生成的代理类中调用业务实现类的同名方法
cglib实现：通过继承业务类，生成的动态代理类是业务类的子类，通过重写业务方法进行代理



# HashMap

HashMap是无序的
Map的有序实现类：TreeMap、LinkedHashMap