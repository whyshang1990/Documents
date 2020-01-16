# Spring Boot 2

## Spring Boot 多环境配置YML

### 在配置文件中切换

在单一文件中，可用连续3个连字号（---）区分多个文件，当配置较少时不用新建多个配置文件

```yaml
# 这些配置为公用配置
spring:
    profgiles:
        active: dev

---
# dev环境采用的配置
spring:
    profiles: dev
server:
  tomcat:
    uri-encoding: utf-8
  port: 8080
---
#当环境为prod时采用该配置
spring:
  profiles: prod
server:
  tomcat:
    uri-encoding: utf-8
  port: 8082
```

### 在不同配置文件中切换

在application.yml中指定环境

```yml
spring:
    profiles:
        active: dev
```

然后在同级目录中新建配置文件 application-dev.yml, 连字符（-）后为环境名称

## 配置日志（logback）

根据不同的日志系统，按照指定的规则组织配置文件名，并放在 resources 目录下，就能自动被 Spring Boot 加载：

- Logback：logback-spring.xml, logback-spring.groovy, logback.xml, logback.groovy
- Log4j: log4j-spring.properties, log4j-spring.xml, log4j.properties, log4j.xml
- Log4j2: log4j2-spring.xml, log4j2.xml
- JDK (Java Util Logging)： logging.properties

想要自定义文件名的可配置：logging.config指定配置文件名: `logging.config=classpath:logging-config.xml`

### logback

logback 配置文件由5部分组成，根节点`<configuration>`有5个子节点，

#### root 

root 节点是必选节点，用来指定最基础的日志输出级别，只有一个 level 属性，用于设置打印级别。

- TRACE
- DEBUG
- INFO
- WARN
- ERROR
- ALL
- OFF

#### contextName

设置上下文名称，默认为default，可通过%contextName来打印上下文名称，一般不使用此属性。

#### property

用于定义变量，方便使用。有2个属性：name，value。定义变量后，使用`${}`来使用变量

```xml
<property name="path" value="./log"/>
...
<File>${LOG_HOME}/timeFile/out.log</File>
...
```

#### appender

appender 用来格式化日志输出，有两个属性

- name 为appender命名，通过name引用appender
- class（指定输出策略，一般有2种，控制台输出、文件输出）
  - ConsoleAppender 把事件添加到控制台
  - FileAppender 把事件添加到文件
  - RollingFileAppender 把事件添加到能够滚动记录的文件（需要配置策略）

##### ConsoleAppender

按照用户指定的编码(encoder)对事件进行格式化。有2个属性：

- encoder 写入控制台输出的格式化方式
- target System.out or System.err。默认为System.out，指定java.io.PrintStream 类型

```xml
<appender name="console" class="ch.qos.logback.core.ConsoleAppender">
    <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
        <!--格式化输出：%d表示日期，%thread表示线程名，%-5level：级别从左显示5个字符宽度%msg：日志消息，%n是换行符-->
        <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{50} - %msg%n</pattern>
        <!--设置编码-->
        <charset>UTF-8</charset>
    </encoder>
</appender>
```

##### FileAppender

新增属性：

- append 如果 true，事件被追加到现存文件尾部。如果 false，清空现存文件。默认为 true。
- file 指定写入文件的文件名，
- prudent

```xml
<timestamp key="bySecond" datePattern="yyyyMMdd'T'HHmmss" />
<appender name="FILE" class="ch.qos.logback.core.FileAppender">
    <file>log-${bySecond}.txt</file>
</appender>
```

##### RollingFileAppender

新增属性：

- rollingPolicy 当发生滚动时，决定 RollingFileAppender 的行为。RollingPolicy 负责滚动步骤，涉及文件移动和重命名。
- triggeringPolicy 告知 RollingFileAppender 何时激活滚动。

###### RollingPolicy

```java
package ch.qos.logback.core.rolling;
import ch.qos.logback.core.FileAppender;
import ch.qos.logback.core.rolling.helper.CompressionMode;
import ch.qos.logback.core.spi.LifeCycle;
public interface RollingPolicy extends LifeCycle {
    // 完成对当前记录文件的归档工作。
    public void rollover() throws RolloverFailure;
    // 计算当前记录文件（写实时记录的地方）的文件名。
    public String getActiveFileName();
    // 决定压缩方式
    public CompressionMode getCompressionMode();
    // 得到一个对其父的引用
    public void setParent(FileAppender appender);
}
```

- FixedWindowRollingPolicy 基于索引来实现(需设置触发策略，指定触发机制)

```xml
<!-- 设置为按照索引的方式滚动，定义文件名称的时候使用%i作为占位符，滚动后会会用角标替换 -->
<rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
    <fileNamePattern>/logback/log/test-%i.log</fileNamePattern>
    <minIndex>1</minIndex>
    <maxIndex>3</maxIndex>
</rollingPolicy>
<!-- 指定文件最大尺寸，达到该尺寸，就触发rollingPolicy对应的策略，maxFileSize属性指定文件大小 -->
<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
    <maxFileSize>1MB</maxFileSize>
</triggeringPolicy>
```

- TimeBasedRollingPolicy 按照时间来滚动

```xml
<appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <prudent>true</prudent>
    <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
        <fileNamePattern>logFile.%d{yyyy-MM-dd}.log</fileNamePattern>
        <maxHistory>30</maxHistory>
    </rollingPolicy>
</appender>
```

###### TriggeringPolicy

```java
package ch.qos.logback.core.rolling;
import java.io.File;
import ch.qos.logback.core.spi.LifeCycle;
public interface TriggeringPolicy<E> extends LifeCycle {
    // activeFile是归档文件，event是当前正被处理的记录事件
    public boolean isTriggeringEvent(final File activeFile, final E event);
}
```

- SizeBasedTriggeringPolicy

#### logger

此节点用来设置一个包或具体的某一个类的日志打印级别、以及指定<appender>，有以下三个属性：

- name: 必须。用来指定受此 logger 约束的某个包或者某个具体的类
- level：可选。设置打印级别。默认为 root 的级别。
- addtivity: 可选。是否向上级 logger(也就是 root 节点)传递打印信息。默认为 true。
  - false：表示只用当前logger的appender-ref，
  - true：表示当前logger的appender-ref和rootLogger的appender-ref都有效

```xml
# name用于指定这个配置对com.example.service包下所有类生效
# level设置打印级别
# appender-ref指定对应的appender
<logger name="com.example.service" level="WARN" addtivity="false">
    <appender-ref ref="console">
</logger>
```

## Spring Data JPA

Spring Data JPA 开发步骤：

- 声明持久层接口，该接口继承Repository的子接口(比如`JpaRepository<DomainClass, PrimayKeyType`>)
`Repository,CrudRepository,PagingAndSortingRepository,JpaRepository`
- 在接口中声明需要的业务方法

创建查询的三种方式

### 通过解析方法名创建查询

### 使用@Query创建查询

在声明的方法上标注，同时提供一个JPQL查询语句

```java
@Query("select a from AccountInfo a where a.accountId = ?1")
public AccountInfo findByAccountId(Long accountId);

@Query("select a from AccountInfo a where a.balance > ?1")
public Page<AccountInfo> findByBalanceGreaterThan(Integer balance,Pageable pageable);
}
// @Query支持命名参数
@Query("select a from AccountInfo a where a.accountId = :id")
public AccountInfo findByAccountId(@Param("id")Long accountId);

@Query("select a from AccountInfo a where a.balance > :balance")
public Page<AccountInfo> findByBalanceGreaterThan(@Param("balance")Integer balance,Pageable pageable);
```

使用更新删除时需标注@Modifying

```java
@Modifying 
@Query("update AccountInfo a set a.salary = ?1 where a.salary < ?2")
public int increaseSalary(int after, int before);
```

### 通过调用JPA命名查询语句创建查询

### 总结

Spring Data Jpa中一共提供了

- Repository：  
提供了findBy + 属性方法  
@Query  
HQL： nativeQuery 默认false  
SQL: nativeQuery 默认true  
更新的时候，需要配合@Modifying使用  
- CurdRepository：继承了Repository  
主要提供了对数据的增删改查
- PagingAndSortRepository：继承了CrudRepository  
提供了对数据的分页和排序，缺点是只能对所有的数据进行分页或者排序，不能做条件判断
- JpaRepository：继承了PagingAndSortRepository  
开发中经常使用的接口，主要继承了PagingAndSortRepository，对返回值类型做了适配
- JpaSpecificationExecutor  
提供多条件查询
