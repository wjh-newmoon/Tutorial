## 一、Spring Boot入门

### 1. pom文件

####	1. 父项目

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

​		标注在哪个类上，那个类就是 Spring Boot 的配置类

​		@**Configration**：配置类上标注该注解

​				配置类  对应  spring 或 springMVC 的配置文件； 配置类也是容器中的一个组件  @Component

​	

@**EnableAutoConfiguration** ；开启自动配置功能

​		以前我们需要配置的东西，加了@**EnableAutoConfiguration** 这个注解，Spring Boot 就会自动帮我们配置。

```java
@AutoConfigurationPackage
@Import({EnableAutoConfigurationImportSelector.class})
public @interface EnableAutoConfiguration {
```

@**AutoConfigurationPackage**： 自动配置包

​		@**Import**({Registrar.class})

​		Spring 的底层注解 @Imort， 给容器导入一个组件；导入的组件由 {Registrar.class} 决定

==将主配置类（@SpringBootApplication标注的类）所在包及下面所有子包里面的所有组件扫描到Spring容器中；==

@**Import({EnableAutoConfigurationImportSelector.class})**

​		该注解决定给容器导入那些组件

​		**EnableAutoConfigurationImportSelector**： 导入那些组件的选择器；

​		将所有需要添加的组件以全类名的方式返回；这些组件会被导入到容器中

​		会给容器导入一些自动配置类（xxxAutoConfiguration）；给容器导入这个场景所需要的组件，并配置好这些组件。

![导入的自动配置类](M:\总结\Tutorial\java\spring\images\2019-11-30_151413.png)

有了自动配置类，就不需要我们手动编写配置 注入组件的工作；

SpringFactoriesLoader.loadFactoryNames( EnableAutoConfiguration.class,classLoader )

==Spring Boot 在启动的时候从类路径下的 META-INF/spring.factories 中获取 EnableAutoConfiguration 指定的值，将这些值对应的类导入到容器中，自动配置类就生效了，不需要我们写配置了。==

J2EE的整体整合解决方案和自动配置都在 spring-boot-autoconfigure-1.5.9.RELEASE.jar 包中





## 二、配置文件

### 1、配置文件

SpringBoot 全局配置文件

- application.properties
- application.yml



配置文件的作用：修改SpringBoot的默认配置

YAML： YAML Ain't Markup Language

标记语言：

- 以前的配置文件大多是 xxx.xml 文件

- YAML 以数据为中心，比json、xml更适合做配置文件

- YAML

  - ```yml
    server:
      port: 8081
    ```

- xml

  - ```xml
    <server>
    	<port>8081</port>
    </server>
    ```

### 2、YAML 语法

#### 1、基本语法

- K:(空格)V

- 空格的缩进控制层级

  （k，v都是大小写敏感）

#### 2、值的写法

##### 字面量：普通的值（数字，字符串，布尔值）

​	k: v 直接写键值对

​	字符串默认不用加双引号或单引号

​	"": 双引号，会转义字符串中的特殊字符

​	'': 单引号，不会转义字符串中的特殊字符



##### 对象、Map、（属性和值）（键值对）

​	在下一行写对象属性和值之间的关系

```yml
friends:
	lastName: lsy
	age: 20
```

行内写法

```yml
friends: {lastName: lsy,age: 20}
```



##### 数组（List，Set）

用 - 表示数组中的每个元素

~~~yml
toys:
  - car
  - computer
  - paper
~~~

行内写法

```yml
toys: [car,computer,paper]
```





### 3、配置文件值注入

配置文件:

```yml
person:
  name: zhangsan
  age: 20
  boss: false
  birthday: 1997/12/12
  maps: {k1: v1,k2: 12}
  lists:
    - lisi
    - zhangsan
  dog:
    name: 达达
    age: 3
```

javaBean:

```java
/**
 * 将配置文件中的值映射到组件中
 * @ConfigurationProperties: 告诉 SpringBoot 将本类中的值与配置文件中的值对应
 *
 * prefix = "person" : 配置文件中那个对应的值下面的属性与此对象的属性一一对应
 *
 * 需要加上 @Component 这个注解，表明这是一个注解，因为只有这个组件是容器中的组件，@ConfigurationProperties 这个注解才能生效
 */
@ConfigurationProperties(prefix = "person")
@Component

public class Person {

    private String name;
    private String age;
    private Boolean boss;
    private Date birthday;

    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;
```



在pom.xml 文件中配置 **文件处理器**， 以后配置属性时就会有提示了。

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-autoconfigure-processor</artifactId>
	<optional>true</optional>
</dependency>
```

#### 1、properties配置文件在idea中默认utf-8可能会乱码

![properties文件乱码调整](M:\总结\Tutorial\java\spring\images\2019-12-01_173633.png)

#### 2、 @Value获取值和@ConfigurationProperties获取值比较

|                           | @ConfigurationProperties | @Value       |
| ------------------------- | ------------------------ | ------------ |
| 功能                      | 批量注入配置文件中的属性 | 一个一个指定 |
| 松散绑定 (松散语法)       | 支持                     | 不支持       |
| SpEL（spring 表达式语言） | 不支持                   | 支持         |
| JSR303数据校验            | 支持                     | 不支持       |
| 复杂类型封装              | 支持                     | 不支持       |

配置文件yml还是properties 都能获取值

==properties文件的优先级比yml文件高==

使用场景：

- 只在业务逻辑中获取一下配置文件中的某项值，使用@**Value**
- 专门编写了一个javaBean来和配置文件进行映射，我们就直接使用@**ConfigurationProperties**

#### 3、配置文件注入值数据校验

```java
@Component
@ConfigurationProperties(prefix = "person")
@Validated

public class Person {

    @Email
    private String name;
    private String age;
    private Boolean boss;
    private Date birthday;

//    @Value("${person.maps}")
    private Map<String, Object> maps;
    private List<Object> lists;
    private Dog dog;

```



































