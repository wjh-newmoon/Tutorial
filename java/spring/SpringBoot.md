## 一、Spring Boot入门

### 1. pom文件

	#### 	1. 父项目

~~~xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starte-parent</artifactId>
    <version>1.5.9.RELEASE</version>
</parent>

他的父项目是
<parent>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-dependencies</artifactId>
  <version>1.5.9.RELEASE</version>
  <relativePath>../../spring-boot-dependencies</relativePath>
</parent>
他来真正管理Spring Boot应用里的所有依赖版本
~~~

Spring Boot 的版本仲裁中心：

以后导入依赖默认不需要写版本；（没有在dependencies里面管理的依赖是需要声明版本号的）



 ####  	2. 启动器

```xml
<dependencies>
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
</dependencies>
```

**spring-boot-starter**- ==**web**==

​		spring-boot-starter: spring-boot场景启动器; 帮我们导入 web 模块正常运行所依赖的组件

> Starters are a set of convenient dependency descriptors that you can include in your application. 
>
> Starters 是一组方便的依赖关系描述符，你可以将其包含在应用程序中。

Spring Boot将所有的功能都抽取出来，做成一个个的starters(启动器)，只需要在项目中引入这些starter，相关场景的所有依赖就都会导入进来。用什么功能就导入什么场景的启动器



### 2. 主程序类，主入口类

```java
/**
 * 用 @SpringBootApplication 来标注一个主程序类，说明这是一个Spring Boot应用
 */

@SpringBootApplication
public class HelloWorldMainApplication {

    public static void main(String[] args) {

        //Spring应用启动起来
        SpringApplication.run(HelloWorldMainApplication.class, args);
    }
}
```

@**SpringBootApplication**: Spring Boot 应用标注在某个类上说明这个类是 SpringBoot 的主配置类，

SpringBoot就应该运行该类的main方法来启动SpringBoot应用

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Inherited
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan(
    excludeFilters = {@Filter(
    type = FilterType.CUSTOM,
    classes = {TypeExcludeFilter.class}
), @Filter(
    type = FilterType.CUSTOM,
    classes = {AutoConfigurationExcludeFilter.class}
)}
)
public @interface SpringBootApplication {
```

@**SpringBootConfiguration**： SpringBoot的配置类；

​		标注在某个类上，表示该类为 Spring Boot 的配置类

​		@**Configration**：配置类上标注该注解

​				配置类  对应  配置文件； 配置类也是容器中的一个组件  @Component

​	

@**EnableAutoConfiguration** ；开启自动配置功能

​		以前我们需要配置的东西，Spring Boot 就会自动帮我们配置； 加了@**EnableAutoConfiguration** 这个注解，Spring Boot就会自动帮我们配置。

```java
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

@**AutoConfigurationPackage**： 自动配置包

​		@**Import**({Registrar.class})

​		Spring 的底层注解 @Imort， 给容器导入一个组件；导入的组件由 {Registrar.class} 决定

==将主配置类（@SpringBootApplication标注的类）所在包及下面所有子包里面的所有组件扫描到Spring容器中；==