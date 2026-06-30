
<!-- more -->

<!-- toc -->

## Java基础常见面试题(仅浅显答案)

### 1. 数组和链表

1. 数组是将元素在内存中连续存放, 通过下标迅速访问, 但是插入元素可能会导致移动大量的元素, 删除同理.
2. 链表的元素在内存中不是顺序排序, 而是通过元素中的指针连在一起, 在访问时只能从第一个开始一个个的走. 但是增加和删除只需要修改元素指针即可.

### 2. 集合类

1. 常见的集合类接口有List和Set, Set是一种不包含重复元素的集合.

2. 常见的List实现有:
   1. ArrayList: 有序可变数组, 允许null, 线程不安全.
   2. LinkedList: 有序链表, 允许null, 线程不安全, 通常用来实现队列(queue)或堆栈(stack).
   3. Vector: 类似ArrayList, 使用synchronized同步, 线程安全.
   4. Stack: 继承自Vector, 实现了一个先进后出的堆栈. 具有基本的push和pop方法.

3. set: 无序列表, 元素不可重复, 否则覆盖. HashSet中不能重复是由HashMap的key实现的, 但本身是线性结构.

4. Map: 键值对的集合, key不允许重复

     1. HashMap: 线程不安全, 数组+链表的结构, 采用链地址法解决哈希碰撞, 初始长度16, 扩容为2倍, 初始长度自定义为奇数则会顺延到最近的一个2的倍数值. JDK1.8后在扩容到64时会转为红黑树.

     2. HashTable: 线程安全的HashMap, 使用synchronized同步, 效率慢. 不允许空键值对.

     3. TreeMap: 线程不安全, 基于红黑树实现, 无调优选项, 因为总处于平衡状态.

     4. ConCurrentHashMap: 线程安全的HashMap, JDK1.7中采用分段锁实现线程安全, Segment继承于ReentrantLock, 理论上支持Segment数组数量的线程并发, 当一个线程占用锁访问一个Segment时, 不会影响其他的Segment. value和链表都是使用volatile修饰, 保证透明度.get的时候不需要加锁.

          <p>JDK1.8中采用cas+synchronized保证并发安全, HashEntry换成了Node, 作用一样. 和HashMap一样也采用了红黑树结构.

### 3. 线程

1. 创建线程的方式

   1. 继承Thread类, run()是方法实现, 启动时用start().

   2. 实现Runnable/Callable接口, Callable有返回值(Future)

   3. 线程池, ThreadPoolExecutor(核心线程数, 最大线程数, 超时时间, 时间单位, 队列, 线程工厂(主要用来创建线程, 比如可以指定线程的名字, 非必填), handler(如果线程池满了, 新任务的处理方式, 非必填)).

      [ThreadPoolExecutor详解](https://blog.csdn.net/Jack_SivenChen/article/details/53394058)

2. java提供的线程池:

   1. newFixedThreadPool: 固定大小的线程池.
   2. newCachedThreadPool: 可缓存线程池, 一般不用, 因为初始化最大线程数是Integer.MAX_VALUE.
   3. newScheduledThreadPool: 定长线程池, 常用于定时及周期性任务执行.
   4. newSingleThreadExecutor: 单线程线程池, 不适用于并发.

3. 阻塞队列

   1. ArrayBlockingQueue: 有边界的阻塞队列, 内部是一个数组, 且初始化后不能改变容量.
   2. DelayQueue: 内部元素必须实现 java.util.concurrent.Delayed接口, 常用于定时关闭链接, 缓存对象, 超时处理等.
   3. **LinkedBlockingQueue(最常用)**: 无边界队列, 由链表实现. 未指定大小时默认为Integer最大值. 所以一般会指定一个值, 否则可能会撑爆JVM.
   4. PriorityBlockingQueue: 无边界队列. 允许插入null对象. 且内部元素必须实现java.lang.Comparable接口.
   5. SynchronousQueue: 单元素队列, 插入一个元素后阻塞, 除非这个元素被消费.

4. 线程的几种状态

   1. 初始(NEW): 新创建了一个线程对象, 但还没调用start()方法.
   2. 运行(RUNNABLE): 就绪(ready)和运行中(running)都被认为是运行状态.
   3. 阻塞(BLOCKED): 阻塞于锁.
   4. 等待(WAITING): 线程等待被唤醒.
   5. 超时等待(TIMED_WAITING): 于等待不同, 可以超时后返回.
   6. 终止(TERMINATED): 表示线程执行完毕.

## Spring常见面试题(仅常见)

### 1. 你对Spring有什么了解(Really? 要这么问?)

1. Spring是一个java企业级应用的轻量级开源开发框架. 简化了java应用开发.
2. Spring的核心是IOC和AOP

### 2. Spring的IOC和AOP

1. IOC是控制反转, Spring通过控制反转实现了松散耦合, 将对象的依赖交由Spring进行管理
2. AOP是面向切面, Spring通过切面把应用业务逻辑和系统服务分开, 比如日志切面.

### 3. 核心容器Application Context(上下文)

​	Application Context是Spring中较高级的容器, 底层也是继承了BeanFactory, 可以加载XML中的bean, 将其集中起来, 在有请求的时候分配bean.

​	常见的实现类有: 

 	1. ClassPathXmlApplicationContext: 默认为项目的classpath相对路径.
 	2. FileSystemXmlApplicationContext: 默认为项目的工作路径, 即项目的根目录.
 	3. XmlWebApplicationContext: 默认为项目的/WEB-INF/目录下.

### 4. AOP是什么, 原理, 配置, 场景

1. AOP把应用分成两个部分: 核心关注点和横切关注点, 业务主要处理的是核心关注点, 与其关系不大的部分是横切关注点. 主要作用在于分离系统中的各种关注点.

2. AOP的原理在于Spring的动态代理, 这样不会对代码进行修改, 而是在内存中生成AOP代理对象, 在特定切点做增强处理, 并回调原对象的方法. 

   <p>代理方式分两种: JDK动态代理和CGLIB动态代理, 不同的是JDK的动态代理是通过反射来接收被代理的类, 且要求和被代理类实现一个接口, 核心是InvocationHandler接口和Proxy类.

   <p>如果目标类没有实现该接口, 则会用CGLIB来动态代理目标类, CGLIB是通过运行时动态生成某个类的子类, 所以如果目标类是final则无法进行代理.

3. 可以通过@Aspect标记切面, 其他的注解有@Pointcut定义切点, @Round, @Before, @After, @AfterReturning, @AfterThrowing. 执行顺序也是如此.
4. AOP的场景可以有事务管理, 日志, 缓存, 权限等.

### 5. IOC

​	IOC负责创建对象, 管理对象(通过DI, 依赖注入), 装配对象, 配置对象, 并管理这些对象的生命周期.

### 6. BeanFactory和Application Context的区别

​	Application Context提供一种方法处理文本消息, 通常表现为加载文件资源. 而且继承了MessageSource接口, 可以实现可插拔的方式提供获取本地化消息的方法.

### 7. Spring框架中的单例bean是线程安全的吗?

​	不是

### 8. @Autowired和@Resource的区别

1. Autowired是由Spring提供, 默认按类型装配, 且要求依赖对象必须存在, 可以通过设置required属性为false来允许null值, 想使用名称装配可以使用@Qualifier注解
2. Resource是JDK1.6支持的注解, 默认按照名称装配.名称可以通过name属性指定, 如果没有则默认去字段名进行名称查找. 两个注解在便利程度上是相同的.

### 9. Spring支持的事务管理类型

1. 编程式事务管理: 可以通过编程的方式管理事务, 灵活但难以维护.
2. 声明式事务管理: 将业务代码和事务管理分开, 只需要注解和XML配置来管理事务.(推荐)

### 10. SpringMVC的请求流程

​	用户发送请求, DispatcherServlet接收请求, 通过映射处理器适配到对应的Handler, 调用响应方法处理, 返回视图, 经过视图解析和渲染返回给用户.

### 11. @Controller和@RestController的区别

​	@RestController相当于@ResponseBody + @Controller, 不能返回页面.

## Spring Boot常见面试题(近常见)

### 1. SpringBoot是什么? 和SpringMVC有什么区别?

​	SpringBoot是由Spring开源组织Pivotal基于Spring开发的开源框架, 是Spring组件的一站式解决方案, 简化了Spring繁琐的配置, 提供了各种启动器. 可以独立运行, 无代码和XML生成, 简化配置, 自动配置等. 

### 2. SpringBoot的核心配置有什么? 区别是什么?

1. application和bootstrap, 可以是yml或者properties. yml不支持@PropertySource注解导入配置.
2. application是主要配置文件, 用于自动配置.
3. bootstrap一般用于Spring Cloud配置, 一些固定不能被覆盖的属性, 一些加密/解密的场景. 加载顺序在application之前.

### 3. SpringBoot的核心注解是哪个? 都包含什么功能?

​	@SpringBootApplication

1. @SpringBootConfiguration, 实现配置文件的功能.
2. @EnableAutoConfiguration, 开启自动配置, 也可以添加属性exclude关闭某个自动配置功能.
3. @ComponentScan, 组件扫描.

### 4. SpringBoot的自动配置原理

​	在MATA-INF下面有Spring.factories文件, 都是需要自动配置的类. 启动时会读取这个文件进行自动配置.

### 5. 怎么在SpringBoot服务启动时加载一些代码

1. @PostContrust, java原生注解, 属于构造器注入, 在类被加载时执行
2. ApplicationRunner接口, 启动获取应用启动时的参数.
3. CommandLineRunner接口, 启动获取命令行参数.
4. 可以通过实现Orderd接口或者@Order注解来实现启动顺序.

### 6. SpringBoot读取配置的方式

1. @PropertySource
2. @Value
3. @Environment
4. @ConfigurationProperties

### 7. 实现热部署的方式

1. Spring Loaded
2. Spring-boot-devtools

## Spring Cloud常见面试题(拙见, 较少)

### 1. SpringCloud和Dubbo的区别

1. SpringCloud在调用方式上使用Rest API, Dubbo使用RPC远程调用, 在微服务中, RPC对于服务提供方和调用方来说依赖太高, 容易出现版本错误, Rest为轻量级接口, 不存在代码之间的耦合, 比RPC更加灵活.
2. 功能上SpringCloud有20多个子项目, Dubbo近实现了服务治理.
3. SpringCloud使用Netflix的Eureka作为注册中心, Dubbo使用ZooKeeper.
4. SpringCloud在社区活跃以及生态上比Dubbo要强的多.

### 2. Eureka和ZooKeeper的区别

1. ZooKeeper保证的是CP(一致性和分区容错性), Eureka保证的是AP(高可用和分区容错)
2. ZooKeeper在选举期间注册服务瘫痪, 虽然服务最终会恢复, 但是选取期间不可用. Eureka各个节点平等, 有一个就可以保证可用, 但是查到的数据不是最新的.

3. Eureka的自我保护机制, ZooKeeper反而会导致整个注册系统瘫痪
   1. Eureka不再从注册列表移除因心跳而应该过期的服务.
   2. Eureka仍然会接受新服务的注册和查询请求, 但不会被同步到其他节点.(高可用)
   3. 当网络稳定时, 新的注册信息会被同步到其他节点.(最终一致性)

### 4. SpringCloud中如何独立通讯

1. 远程过程调用(RPI), 也就是服务的注册和发现, 直接通过远程过程调用访问别的Service
   1. 优点: 简单, 没有中间件代理, 系统更简洁.
   2. 缺点: 只支持请求/响应, 不支持比如通知, 异步响应, 发布/订阅, 订阅的异步响应.
2. 消息, 使用异步消息通讯, 服务和服务之间通过消息管道.
   1. 优点: 可用性高, 也支持请求/响应, 不支持比如通知, 异步响应, 发布/订阅, 订阅的异步响应等.
   2. 缺点: 消息中间件的额外复杂性.

### 5. 服务熔断和服务降级

1. 服务降级一般指服务在高并发下产生了阻塞, 导致当前线程不可用, 服务器的线程全部堵塞, 导致服务器的崩溃.

   当某个服务调用响应时间过长或不可用占用资源达到一定阈值, 根据业务进行一些策略上的不处理或者简单处理, 释放资源保证核心服务的正常运转.

2. 常见的降级情况有: 超时降级, 失败次数降级, 故障降级, 限流降级.

3. 熔断一般指某个服务故障或者异常等异常条件时, 熔断器打开, 直接熔断整个服务, 进入指定的熔断逻辑, 在请求时直接返回fallback的值, 而不是一直等待超时或者一直报错. 经过一段时间后, 熔断器会进入半开状态, 允许通过一个请求, 当调用成功时, 熔断器恢复关闭状态, 请求失败则继续保持打开, 直到下一个半开时间.

### 6. 微服务技术栈都有哪些?

1. 服务开发: Spring, SpringBoot, SpringMVC
2. 注册中心: Netflix-Eureka, ZooKeeper

3. 服务调用: Rest, RPC
4. 熔断: Hystrix
5. 负载均衡: Ribbon, Nginx
6. 服务接口调用: Dubbo, Feign
7. 消息队列: Kafka, RabbitMQ等
8. 配置中心: SpringCloudConfig
9. 网关: Zuul
10. 消息总线: SpringCloudBus

## Dubbo常见面试题



## Netty常见面试题




## Mybatis常见面试题(都是一些常见简单的)

### 1. #{}和${}的区别

​	\#{}解析传递进来的参数数据, ${}则是原样拼接.

​	#{}是预编译处理, 可以有效的防止SQL注入, ${}是字符串替换.

### 2. 当实体类中的属性名和表中的字段名不一样

1. 通过在查询的sql语句中定义字段名的别名, 使其和实体类的属性名一致.
2. 通过\<resultMap>做字段映射(一般是首选)

### 3. 如何在插入后获取主键

​	通过selectKey, 执行select LAST_INSERT_ID()获取

### 4. 在mapper中如何传递多个参数

1. \#{0}, #{1}按照顺序指定(不推荐)

 	2. 通过@Param(name), @Param(code)指定#{name}, #{code}

3. 通过map, 用法同2, 需指定parameterType="map"

### 5. 动态sql, 都有哪些标签, 执行原理

1. 动态sql是在XML映射文件或者代码中, 通过条件判断动态拼接出来的sql.
2. Mybatis提供了9种动态sql标签: trim, where, set, foreach, if, choose, when, otherwise, bind
3. 执行原理是使用OGNL从sql参数对象中计算表达式的值, 再根据值动态拼接sql.

### 6. 在xml映射文件中, 不同的xml映射文件id是否可以重复

​	如果配置了namespace可以.

### 7. dao接口和xml的映射工作原理是什么? 能否重载?

1. dao接口(mapper接口)的全限名, 就是映射文件的namespace,

   <p>接口中的方法就是映射文件中MappedStatement的id值,

   <p>接口方法中的参数就是传递给sql的参数.

   工作原理是JDK的动态代理, 运行时会为接口生成代理对象, 代理对象拦截接口方法, 转而执行MappedStatement所代表的sql, 然后返回结果.

2. 不可以重载. 因为全限名+方法名是映射的寻找策略.

### 8. Mybatis的分页和分页插件的分页有什么区别?

1. Mybatis的分页是通过针对ResultSet结果集执行的内存分页.
2. 分页插件是通过Mybatis提供的插件接口, 拦截sql进行物理分页的参数拼接(limit x, y).



## MySQL常见面试题

### 1. 常见的引擎和区别

1. InnoDB和MyISAM
2. InnoDB支持事务, MyISAM不支持, InnoDB执行每一条sql都会默认封装成事务自动提交.
3. InnoDB支持外键, MyISAM不支持, 包含外键的InnoDB表转为MyISAM会失败.
4. InnoDB是聚集索引, 数据文件和索引是绑在一起的, 必须有主键. 索引查询也会先查询到主键, 再通过主键查询到数据. 所以主键不应该太大, 会导致其他索引也很大. MyISAM是非聚集索引, 索引是保存数据文件的指针, 主键和其他索引是独立的.
5. InnoDB不保存表的行数, count时为全表扫描. MyISAM用一个变量保存了整个表的行数, count时直接返回.
6. InnoDB不支持全文索引, MyISAM支持, 所以在查询效率上MyISAM更高.
7. InnoDB提供行锁, MyISAM不提供.

### 2. 意向锁

1. 首先InnoDB引擎支持行锁, 行锁分为:
   1. 共享锁: 一个事务读一行的数据, 阻止其他事务获得这一行数据的排他锁.
   2. 排他锁: 一个事务对一行数据进行修改, 阻止其他事务获得这一行数据的共享锁和排他锁.
2. 当一个读取的事务A需要读a行, 在数据库内部会判断在给a行加共享锁之前先取得**意向共享锁**.
3. 当一个修改的事务B需要修改b行, 在数据库内部会判断给b行加排他锁之前先取得**意向排他锁**.
4. InnoDB的锁是通过索引上的索引项实现的, 当使用非索引条件检索数据则会变为表锁.
5. 意向锁可以通过显式的在select语句后面加LOCK IN SHARE MODE和FOR UPDATE进行加锁. 但注意事务不要造成死锁.

### 3. MySQL的事务特征和隔离级别

1. 事务具有4个特征: 
   1. 原子性(A): 事务中要么都成功, 要么都失败.
   2. 一致性(C): 事务的执行结果必须是从一个一致性状态变到另一个一致性状态. 比如转账的前后金额变化.
   3. 隔离性(I): 事务的执行不能受到其他事务的干扰.
   4. 持久性(D): 事务一旦提交, 对数据库的改变就是永久性的.
2. 隔离级别有4种:
   1. 读未提交: 脏读.
   2. 读已提交: 大部分数据库的默认隔离级别, 但不是MySQL的. 也叫不可重复读.
   3. 可重复读: **MySQL的默认隔离级别**. 会出现幻读. 使用其作为默认隔离级别主要因为语句级的Binlog, 详情请自行查询.
   4. 串行化: 强制排序执行.

### 4. 索引

1. 实现方式有BTREE(默认)和HASH.
2. 类型分三种:
   1. 普通索引: 无任何约束.
   2. 唯一索引: 具有唯一性约束. 可有多个唯一索引.
   3. 主键索引: 特殊的唯一索引, 不允许有空值. 一个表只能有一个主键索引.
   4. 复合索引: 多个列组合一起创建索引, 可以是普通也可以是唯一, 覆盖多个列.
3. 创建原则:
   1. 最适合的列是出现在where或on字句中的列, 或者连接字句中的列. 而不是出现在select关键字后面的列.
   2. 索引列的基数越大, 数据区分度越高, 效果越好.
   3. 根据情况创建复合索引, 更好的提高查询效率.
   4. 避免创建过多的索引, 会额外占用磁盘空间, 降低写操作的效率. 
   5. 主键尽量选择较短的数据类型.
4. 索引失效的情况:
   1. 复合索引的乱序使用.
   2. LIKE关键字, %不能在前, 可用position代替.
   3. null值其实会走索引, 但是会导致索引和索引统计更复杂.
   4. 当数据库判断使用索引比全表扫描还慢时, 比如where id>1 and id<100
   5. or关键字前面的条件中的列有索引, 后面的没有, 则都不会生效.
   6. 字符串类型的列在查询时一定要加引号, 否则不会走索引.
   7. 索引上做计算或函数或类型转换.
   8. 范围条件(between, <, >)右边的列将不走索引, 比如where age>10 and name = 'tom'
   9. <,>,!= 无法走索引.
   10. is null,  is not null无法走索引
5. 优化
   1. 用exists代替in
   2. 避免在where字句中进行表达式或者子查询
   3. or可以换成union.
   4. in的列表中, 将出现最频繁的放在前面, 可以减少判断次数.
   5. 尽量使用>=而不是>
   6. [更多的查看这里](https://zhuanlan.zhihu.com/p/48385127).

### 5. SQL编写

> 学生表(Student), 课程表(Course), 教师表(Teacher), 成绩表(Score)

1. 查询平均成绩大于60分的学生学号和平均成绩

   ```sql
   select s.sno as '学号', avg(s.score) as '平均成绩' from Score s group by s.sno having avg(s.score)>60
   ```

2. 查询至少选修两门课的学生学号

   ```sql
   select s.sno from Score s group by s.sno having count(s.sno)>=2
   ```

3. [其他参考](https://zhuanlan.zhihu.com/p/38354000)


