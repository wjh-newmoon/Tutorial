### 一、日志

#### 1、什么是日志

能够描述系统运行状态的事件都可以称为日志

#### 2、什么是日志框架

一套能实现日志输出的工具包



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

#### 数据库配置 yml文件

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

