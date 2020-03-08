

### 一、日志

#### 1、什么是日志

能够描述系统运行状态的事件都可以称为日志

#### 2、什么是日志框架

一套能实现日志输出的工具包

#### 3、日志输出

~~~java
package com.wjh.sell;

import lombok.extern.slf4j.Slf4j;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest

//lombok中的注解
@Slf4j
public class LoggerTest {

    @Test
    public void test1(){
        log.debug("debug...");
        log.error("error...");
        log.info("info...");
    }
}
~~~

#### @Slf4j

使用该注解默认生成如下代码

~~~java
import org.slf4j.Logger;

private static final Logger log = org.slf4j.LoggerFactory.getLogger(当前类名.class);
~~~

#### 日志中插入变量

~~~java
String name = "wjh";
String age = "18";
log.info("name: {}, age: {}", name, age);
~~~

#### 4、日志配置

**application.yml :**  只能进行一些简单的配置  比如：**日志文件路径**，**日志输出格式**

**logback-spring.xml :**  可以进行一些较为复杂的配置

application.yml

~~~yml
logging:
  pattern:
    console: "%d - %msg%n" #日志输出格式

  file:
    path: M:/logs/tomcat/ #日志导出路径
~~~

logback-spring.xml

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="consoleLog" class="ch.qos.logback.core.ConsoleAppender">
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>
                %d - %msg%n #控制台日志格式
            </pattern>
        </layout>
    </appender>

    <appender name="fileInfoLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <level>ERROR</level>
            <onMatch>DENY</onMatch>
            <onMismatch>ACCEPT</onMismatch>
        </filter>
        <encoder>
            <pattern>
                %msg%n #输入到文件中的日志格式
            </pattern>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>M:/logs/tomcat/info.%d.log</fileNamePattern>
        </rollingPolicy>
    </appender>

    <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <encoder>
            <pattern>
                %msg%n
            </pattern>
        </encoder>
        <!--滚动策略-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--路径-->
            <fileNamePattern>M:/logs/tomcat/error.%d.log</fileNamePattern>
        </rollingPolicy>
    </appender>

    <root level="info">
        <appender-ref ref="consoleLog" />
        <appender-ref ref="fileInfoLog" />
        <appender-ref ref="fileErrorLog" />
    </root>
</configuration>
~~~

别人博客借鉴

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration scan="true" scanPeriod="60 seconds" debug="false">
 
    <!--<contextName>logback-demo</contextName>-->
    <property name="LOG_HOME" value="${user.dir}/logs/" />
    <!--<property name="LOG_HOME" value="../logs/" />-->
 
    <property name="appName" value="demolog"></property>
 
    <!--输出到控制台 ConsoleAppender-->
    <appender name="consoleLog1" class="ch.qos.logback.core.ConsoleAppender">
        <!--展示格式 layout-->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </layout>
    </appender>
 
    <appender name="fileInfoLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!--如果只是想要 Info 级别的日志，只是过滤 info 还是会输出 Error 日志，因为 Error 的级别高，
        所以我们使用下面的策略，可以避免输出 Error 的日志-->
        <filter class="ch.qos.logback.classic.filter.LevelFilter">
            <!--过滤 Error-->
            <level>ERROR</level>
            <!--匹配到就禁止-->
            <onMatch>DENY</onMatch>
            <!--没有匹配到就允许-->
            <onMismatch>ACCEPT</onMismatch>
        </filter>
        <!--日志名称，如果没有File 属性，那么只会使用FileNamePattern的文件路径规则
            如果同时有<File>和<FileNamePattern>，那么当天日志是<File>，明天会自动把今天
            的日志改名为今天的日期。即，<File> 的日志都是当天的。
        -->
        <File>${LOG_HOME}/info.${appName}.log</File>
        <!-- <File>info.log</File>-->
        <!--滚动策略，按照时间滚动 TimeBasedRollingPolicy-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--文件路径,定义了日志的切分方式——把每一天的日志归档到一个文件中,以防止日志填满整个磁盘空间-->
            <FileNamePattern>${LOG_HOME}/info.${appName}.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--只保留最近90天的日志-->
            <maxHistory>90</maxHistory>
            <!--用来指定日志文件的上限大小，那么到了这个值，就会删除旧的日志-->
            <!--<totalSizeCap>1GB</totalSizeCap>-->
        </rollingPolicy>
        <!--日志输出编码格式化-->
        <encoder>
            <charset>UTF-8</charset>
            <pattern>%d [%thread] %-5level %logger{36} %line - %msg%n</pattern>
        </encoder>
    </appender>
 
 
    <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!--如果只是想要 Error 级别的日志，那么需要过滤一下，默认是 info 级别的，ThresholdFilter-->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>Error</level>
        </filter>
        <!--日志名称，如果没有File 属性，那么只会使用FileNamePattern的文件路径规则
            如果同时有<File>和<FileNamePattern>，那么当天日志是<File>，明天会自动把今天
            的日志改名为今天的日期。即，<File> 的日志都是当天的。
        -->
        <!-- <File>${logback.logdir}/error.${logback.appname}.log</File>-->
        <File>${LOG_HOME}/error.${appName}.log</File>
        <!--滚动策略，按照时间滚动 TimeBasedRollingPolicy-->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!--文件路径,定义了日志的切分方式——把每一天的日志归档到一个文件中,以防止日志填满整个磁盘空间-->
            <FileNamePattern>${LOG_HOME}/error.${appName}.%d{yyyy-MM-dd}.log</FileNamePattern>
            <!--只保留最近90天的日志-->
            <maxHistory>90</maxHistory>
            <!--用来指定日志文件的上限大小，那么到了这个值，就会删除旧的日志-->
            <!--<totalSizeCap>1GB</totalSizeCap>-->
        </rollingPolicy>
        <!--日志输出编码格式化-->
        <encoder>
            <charset>UTF-8</charset>
            <pattern>%d [%thread] %-5level %logger{36} %line - %msg%n</pattern>
        </encoder>
    </appender>
 
    <!--指定最基础的日志输出级别-->
    <root level="INFO">
        <!--appender将会添加到这个loger-->
        <appender-ref ref="consoleLog1"/>
        <appender-ref ref="fileErrorLog"/>
        <appender-ref ref="fileInfoLog"/>
    </root>
 
</configuration>
~~~





### 二、实战

#### @Transactional

单元测试时，用这个注解，数据库的操作会完全回滚

#### @Data

实例类中，不用写setter，getter，toString 等方法，**lombok** jar包中的注解

~~~xml
<dependency>
	<groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>
~~~

#### @DynamicUpdate

实体类中，时间自动更新

#### 数据库相关

- **数据库相关配置**（yml文件）

~~~yml
spring:
	datasource:
		driver-class-name: com.mysql.jdbc.Driver
		username: root
		password: 123456
		url: jdbc:mysql://192.168.0.113/sell?characterEncoding=utf-8&useSSL=false
	jpa:
		show-sql: true	
~~~

- **jpa依赖导入**（pom.xml)

~~~xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
~~~

- **mysql-connector-java 依赖导入**（pom.xml)

~~~xml
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
</dependency>
~~~

- **JPA相关**

  - **JPA中DAO层代码**

  ~~~java
  package com.wjh.sell.repository;
  
  import com.wjh.sell.dataObject.ProductCategory;
  import org.springframework.data.jpa.repository.JpaRepository;
  
  public interface ProductCategoryRepository extends JpaRepository<ProductCategory, Integer> {
  
  }
  ~~~

  

  - **JPA单元测试**

~~~java
package com.wjh.sell.repository;

import com.wjh.sell.dataObject.ProductCategory;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;

@RunWith(SpringRunner.class)
@SpringBootTest

public class ProductCategoryRepositoryTest {
    @Autowired
    private ProductCategoryRepository repository;

    @Test
    public void findOneTest() {
        ProductCategory productCategory = repository.findById(1).get();
        System.out.println(productCategory.toString());
    }
}

~~~

