第1章Spring_day03笔记
1.1上次课内容回顾
Spring的IOC的注解开发
注解的入门
引入aop的包
引入context约束
<context:component-scan />
使用注解开发
@Component		：定义Bean
@Controller	：WEB层
@Service		：Service层
@Repository	：DAO层
属性注入：
普通属性	：@Value
对象属性	：@Resource
@Autowired	：按类型注入属性，按名称@Qulifier
XML方式和注解方式比较
XML方式	：适用性更广，结构更加清晰。
注解方式	：适用类是自己定义，开发更方便。
XML和注解的整合开发
XML定义类
注解属性注入
Spring的AOP的基于AspectJ的XML的开发
AOP的概述
AOP：面向切面编程，是OOP的扩展和延伸，是用来解决OOP遇到问题。
Spring的AOP
底层的实现
JDK的动态代理
Cglib的动态代理
AOP的相关术语
连接点：可以被拦截的点。
切入点：真正被拦截的点。
通知：增强方法
引介：类的增强
目标：被增强的对象
织入：将增强应用到目标的过程。
代理：织入增强后产生的对象
切面：切入点和通知的组合
AOP的入门开发
引入jar包
编写目标类并配置
编写切面类并配置
进行aop的配置
<aop:config>
	<aop:pointcut expression=”execution(表达式)” id=”pc1”/>
<aop:aspect >
	<aop:before method=”” pointcut-ref=”pc1”/>
</aop:aspect>
</aop:config>
通知类型
前置通知
后置通知
环绕通知
异常抛出通知
最终通知
切入点表达式写法
1.2Spring的AOP的基于AspectJ注解开发
1.2.1Spring的基于ApsectJ的注解的AOP开发
1.2.1.1创建项目，引入jar包

1.2.1.2引入配置文件
1.2.1.3编写目标类并配置

1.2.1.4编写切面类并配置


1.2.1.5使用注解的AOP对象目标类进行增强
在配置文件中打开注解的AOP开发

在切面类上使用注解

1.2.1.6编写测试类

1.2.2Spring的注解的AOP的通知类型
1.2.2.1@Before	：前置通知

1.2.2.2@AfterReturning	：后置通知

1.2.2.3@Around		：环绕通知

1.2.2.4@AfterThrowing		：异常抛出通知

1.2.2.5@After		：最终通知

1.2.3Spring的注解的AOP的切入点的配置
1.2.3.1Spring的AOP的注解切入点的配置



1.3Spring的JDBC的模板的使用
1.3.1Spring的JDBC的模板
Spring是EE开发的一站式的框架，有EE开发的每层的解决方案。Spring对持久层也提供了解决方案：ORM模块和JDBC的模板。
Spring提供了很多的模板用于简化开发：

1.3.1.1JDBC模板使用的入门
创建项目，引入jar包
引入基本开发包：
数据库驱动
Spring的JDBC模板的jar包

1.3.1.2创建数据库和表
create database spring4_day03;
use spring4_day03;
create table account(
	id int primary key auto_increment,
	name varchar(20),
	money double
);
1.3.1.3使用JDBC的模板：保存数据


1.3.2将连接池和模板交给Spring管理
1.3.2.1引入Spring的配置文件

1.3.2.2使用Jdbc的模板
引入spring_aop的jar包

1.3.3使用开源的数据库连接池：
1.3.3.1DBCP的使用
引入jar包

配置DBCP连接池

1.3.3.2C3P0的使用
引入c3p0连接池jar包

配置c3p0连接池

1.3.4抽取配置到属性文件
1.3.4.1定义一个属性文件

1.3.4.2在Spring的配置文件中引入属性文件
第一种：

第二种：

1.3.4.3引入属性文件的值

1.3.4.4测试

1.3.5使用JDBC的模板完成CRUD的操作
1.3.5.1保存操作

1.3.5.2修改操作

1.3.5.3删除操作

1.3.5.4查询操作
查询某个属性

查询返回对象或集合

数据封装

1.4Spring的事务管理
1.4.1事务的回顾
1.4.1.1什么事务
事务：逻辑上的一组操作，组成这组操作的各个单元，要么全都成功，要么全都失败。
1.4.1.2事务的特性
原子性：事务不可分割
一致性：事务执行前后数据完整性保持一致
隔离性：一个事务的执行不应该受到其他事务的干扰
持久性：一旦事务结束，数据就持久化到数据库
1.4.1.3如果不考虑隔离性引发安全性问题
读问题
脏读		：一个事务读到另一个事务未提交的数据
不可重复读	：一个事务读到另一个事务已经提交的update的数据，导致一个事务中多次查询结果不一致
虚读、幻读	：一个事务读到另一个事务已经提交的insert的数据，导致一个事务中多次查询结果不一致。
写问题
丢失更新
1.4.1.4解决读问题
设置事务的隔离级别
Read uncommitted	：未提交读，任何读问题解决不了。
Read committed	：已提交读，解决脏读，但是不可重复读和虚读有可能发生。
Repeatable read	：重复读，解决脏读和不可重复读，但是虚读有可能发生。
Serializable		：解决所有读问题。
1.4.2Spring的事务管理的API
1.4.2.1PlatformTransactionManager：平台事务管理器
平台事务管理器：接口，是Spring用于管理事务的真正的对象。
DataSourceTransactionManager	：底层使用JDBC管理事务
HibernateTransactionManager	：底层使用Hibernate管理事务
1.4.2.2TransactionDefinition	：事务定义信息
事务定义：用于定义事务的相关的信息，隔离级别、超时信息、传播行为、是否只读
1.4.2.3TransactionStatus：事务的状态
事务状态：用于记录在事务管理过程中，事务的状态的对象。
1.4.2.4事务管理的API的关系：
Spring进行事务管理的时候，首先平台事务管理器根据事务定义信息进行事务的管理，在事务管理过程中，产生各种状态，将这些状态的信息记录到事务状态的对象中。
1.4.3Spring的事务的传播行为
1.4.3.1Spring的传播行为
Spring中提供了七种事务的传播行为：
保证多个操作在同一个事务中
PROPAGATION_REQUIRED		：默认值，如果A中有事务，使用A中的事务，如果A没有，创建一个新的事务，将操作包含进来
PROPAGATION_SUPPORTS		：支持事务，如果A中有事务，使用A中的事务。如果A没有事务，不使用事务。
PROPAGATION_MANDATORY	：如果A中有事务，使用A中的事务。如果A没有事务，抛出异常。

保证多个操作不在同一个事务中
PROPAGATION_REQUIRES_NEW		：如果A中有事务，将A的事务挂起（暂停），创建新事务，只包含自身操作。如果A中没有事务，创建一个新事务，包含自身操作。
PROPAGATION_NOT_SUPPORTED	：如果A中有事务，将A的事务挂起。不使用事务管理。
PROPAGATION_NEVER				：如果A中有事务，报异常。

嵌套式事务
PROPAGATION_NESTED			：嵌套事务，如果A中有事务，按照A的事务执行，执行完成后，设置一个保存点，执行B中的操作，如果没有异常，执行通过，如果有异常，可以选择回滚到最初始位置，也可以回滚到保存点。
1.4.4Spring的事务管理
1.4.4.1搭建Spring的事务管理的环境
创建Service的接口和实现类

创建DAO的接口和实现类

配置Service和DAO：交给Spring管理

在DAO中编写扣钱和加钱方法：
配置连接池和JDBC的模板

在DAO注入Jdbc的模板：

测试

1.4.5Spring的事务管理：一类：编程式事务（需要手动编写代码）--了解
1.4.5.1第一步：配置平台事务管理器

1.4.5.2第二步：Spring提供了事务管理的模板类
配置事务的管理的模板类

1.4.5.3第三步：在业务层注入事务管理的模板

1.4.5.4编写事务管理的代码

1.4.5.5测试：

1.4.6Spring的事务管理：二类：声明式事务管理（通过配置实现）---AOP
1.4.6.1XML方式的声明式事务管理
第一步：引入aop的开发包
第二步：恢复转账环境
第三步：配置事务管理器

第四步：配置增强

第五步：AOP的配置

测试

1.4.6.2注解方式的声明式事务管理
第一步：引入aop的开发包
第二步：恢复转账环境
第三步：配置事务管理器

第四步：开启注解事务

第五步：在业务层添加注解

