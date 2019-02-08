第1章Spring_day04笔记
1.1上次课内容回顾
Spring的AOP的注解（思想--）
AOP的相关的注解
@Aspect		：定义切面
通知的注解
@Before			：前置通知
@AfterReturning	：后置通知
@Around			：环绕通知
@AfterThrowing	：异常抛出通知
@After			：最终通知
切入点
@Pointcut		：切入点
Spring的JDBC模板
Spring有持久层解决方案
手动使用JDBC模板
将连接池和JDBC模板交给Spring管理
配置开源连接池
DBCP
C3P0
使用JDBC模板完成CRUD的操作
Spring的事务管理
事务回顾
Spring的事务管理的API
PlatformTransactionManager		：平台事务管理器（真正管理事务）
DataSourceTransactionManager
HibernateTransactionManager
TransactionDefinition				：事务定义信息
TransactionStatus					：事务状态
Spring事务管理
编程式事务管理（了解）
声明式事务管理---底层思想就是AOP
XML
引入aop开发包
配置事务管理器
配置增强<tx:Advice>
配置aop
注解
引入aop开发包
配置事务管理器
开启注解事务
在业务层添加注解
1.2SSH整合方式一：无障碍整合
1.2.1SSH框架回顾
1.2.1.1SSH框架整合开发回顾

1.2.2SSH整合
1.2.2.1第一步：创建web项目，引入jar包
Struts2的jar包
struts-2.3.24\apps\struts2-blank\WEB-INF\lib\*.jar
Struts2中有一些包需要了解的：
struts2-convention-plugin-2.3.24.jar			----Struts2的注解开发包。
struts2-json-plugin-2.3.24.jar				----Struts2的整合AJAX的开发包。
struts2-spring-plugin-2.3.24.jar				----Struts2的整合Spring的开发包。
Hibernate的jar包
Hibernate的开发的必须的包	
hibernate-release-5.0.7.Final\lib\required\*.jar
MySQL驱动
日志记录

使用C3P0连接池：

*****注意：Struts2和Hibernate都引入了一个相同的jar包（javassist包）。删除一个******
Spring的jar包
IOC的开发

AOP的开发

JDBC模板的开发

事务管理

整合web项目的开发

整合单元测试的开发

整合hibernate的开发

1.2.2.2第二步：引入配置文件
Struts的配置文件
web.xml

struts.xml
Hibernate的配置文件
hibernate.cfg.xml
删除那个与线程绑定的session。
映射文件
Spring的配置文件
web.xml

applicationContext.xml

日志记录
log4j.properties
1.2.2.3第三步：创建包结构

1.2.2.4第四步：创建相关类

1.2.2.5第五步：引入相关的页面
1.2.2.6第六步：修改add.jsp

1.2.2.7第七步：Spring整合Struts2方式一：Action由Struts2自身创建的。
编写Action

配置Action
在struts.xml中配置

在Action中引入Service
传统方式

进行Spring和Struts2的整合：
引入struts-spring-plugin.jar
在插件包中有如下配置

*****开启一个常量：在Struts2中只有开启这个常量就会引发下面常量生效：

让Action按照名称自动注入Service：
将Service交给Spring管理

Action注入Service

1.2.2.8第八步：Spring整合Struts2方式二：Action交给Spring管理（推荐）
引入插件包
引入struts-spring-plugin.jar
将Action交给Spring

在struts.xml中配置Action

注意：
需要配置Action为多例的：

需要手动注入Service

1.2.2.9第九步：Service调用DAO
将DAO交给Spring管理

在Service注入DAO


1.2.2.10第十步：Spring整合Hibernate框架
创建数据库和表
Create database ssh1;
Use ssh1;
CREATE TABLE `cst_customer` (
  `cust_id` bigint(32) NOT NULL AUTO_INCREMENT COMMENT '客户编号(主键)',
  `cust_name` varchar(32) NOT NULL COMMENT '客户名称(公司名称)',
  `cust_source` varchar(32) DEFAULT NULL COMMENT '客户信息来源',
  `cust_industry` varchar(32) DEFAULT NULL COMMENT '客户所属行业',
  `cust_level` varchar(32) DEFAULT NULL COMMENT '客户级别',
  `cust_phone` varchar(64) DEFAULT NULL COMMENT '固定电话',
  `cust_mobile` varchar(16) DEFAULT NULL COMMENT '移动电话',
  PRIMARY KEY (`cust_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
编写实体和映射
Spring和Hibernate整合
在Spring的配置文件中，引入Hibernate的配置的信息

在Spring和Hibernate整合后，Spring提供了一个Hibernate的模板类简化Hibernate开发。
改写DAO继承HibernateDaoSupport

配置的时候在DAO中直接注入SessionFactory

在DAO中使用Hibernate的模板完成保存操作


1.2.2.11第十一步：配置Spring的事务管理
配置事务管理器

开启注解事务

在业务层使用注解


1.3SSH整合方式二：将hibernate的配置交给Spring管理
1.3.1SSH整合方式二：不带hibernate配置文件
1.3.1.1复制一个项目
1.3.1.2hibernate配置文件中有哪些内容：
数据库连接的配置
Hibernate的相关的属性的配置
方言
显示SQL
格式化SQL
。。。
C3P0连接池
映射文件
1.3.1.3将Hibernate的配置交给Spring
	<!-- 引入外部属性文件=============================== -->
	<context:property-placeholder location="classpath:jdbc.properties"/>
	
	<!-- 配置C3P0连接池=============================== -->
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="${jdbc.driverClass}"/>
		<property name="jdbcUrl" value="${jdbc.url}"/>
		<property name="user" value="${jdbc.username}"/>
		<property name="password" value="${jdbc.password}"/>
	</bean>
	
	<!-- Spring整合Hibernate -->
	<!-- 引入Hibernate的配置的信息=============== -->
	<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
		<!-- 注入连接池 -->
		<property name="dataSource" ref="dataSource"/>
		<!-- 配置Hibernate的相关属性 -->
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
				<prop key="hibernate.show_sql">true</prop>
				<prop key="hibernate.format_sql">true</prop>
				<prop key="hibernate.hbm2ddl.auto">update</prop>
			</props>
		</property>
		
		<!-- 设置映射文件 -->
		<property name="mappingResources">
			<list>
				<value>com/itheima/ssh/domain/Customer.hbm.xml</value>
			</list>
		</property>
	</bean>

1.4Hibernate的模板的使用
1.4.1Hibernate模板的常用的方法
1.4.1.1保存操作
save(Object obj);
1.4.1.2修改操作
update(Object obj);
1.4.1.3删除操作
delete(Object obj);
1.4.1.4查询操作
查询一个
get(Class c,Serializable id);
load(Class c,Serializable id);
查询多个
List find(String hql,Object… args);
List findByCriteria(DetachedCriteria dc);
List findByCriteria(DetachedCriteria dc,int firstResult,int maxResults);
List findByNamedQuery(String name,Object… args);
1.5延迟加载问题的解决
1.5.1Spring提供了延迟加载的解决方案
1.5.1.1在SSH整合开发中哪些地方会出现延迟加载
使用load方法查询某一个对象的时候（不常用）
查询到某个对象以后，显示其关联对象。
