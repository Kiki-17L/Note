# Spring 学习记录

## 装配

### 1. 什么是 ___装配___ (___wiring___)?

>
> 创建应用组件之间协作的行为，通常称为**装配**（___wiring___）
>

### 2. 三种装配**机制** 

>
>    * #### XML显示装配
>
>    * #### 在Java中进行显示装配
>
>      > 通过**@Configuration** 注解声明一个**Spring配置类**。
>      >
>      > 
>      >
>      > ###### 组件命名
>      >
>      > * 通过**@Component( beanID )**注解给组件设置**ID**
>      >
>      > * 通过**@Named( beanID )**注解设置组件**ID** (这个**@Named**注解是在**Java依赖注入规范**中定义的，不是Spring框架特有的)。
>      >
>      > * 通过**@Bean( name="")**注解的name属性设置组件ID
>      >
>      > > **JavaConfig**配置类中，**bean**的ID默认与**@Bean**注解的方法名一致。
>      > >
>      > > ```java
>      > > @Bean(name="test")				//这个Bean默认的ID是方法名knight,但是在@Bean注解中设置了name值为test
>      >   > public Knight knight(){			//此时BeanID就是test
>      >   >     
>      >   >     return new Knight()
>      >   > }
>      >   > ```
>      >   > 
>      > ##### JavaConfig实现注入
>      >
>      > > ```java
>      > > //若Quest依赖Knight,则借助@Bean注解方法knight实现注入
>      > > @Bean
>      > > public Quest quest(){
>      > >     
>      > >     return new Quest(knight())
>      > > }
>      > > ```
>
>    * #### 隐式的bean发现机制和自动装配
>
>      > ##### 启用组件扫描的方式
>      >
>      > 
>      >
>      > * 隐式装配Config类需要使用**@ComponentScan**注解，**@ComponentScan**默认会扫描与 ***配置类相同的包***，在这个包及其子包下，查找带有**@Component**注解的类，并且会在Spring中自动为其创建一个___Bean___
>      >
>      >   > ```java
>      >   > @Configuration
>      >   > @ComponentScan
>      >   > public class Config{
>      >   > 
>      >   > }
>      >   > ```
>      >   >
>      >   > 同时，可以通过**@ComponentScan( packageName )**注解，设置组件扫描的**基础包**。
>      >   >
>      >   > 如果需要配置多个**基础包**，则设置**@ComponentScan( basePackages={"","",""})**的属性basePackages的值为一个包名数组。
>      >   >
>      >   > 然而，由于basePackages值为**String**类型，显然，只是类型不安全的 (**not type safe**)。
>      >   >
>      >   > 可以通过设置**basePackageClasses**属性，传入一个或多个**Class**对象，基础包将被设置为这些类所在的包。
>      >
>      > * 显示通过__配置XML__，启动组件扫描
>      >
>      >   > ```xml
>      >   > <context:component-scan base-package="...."/>  
>      >   > ```
>      >   >
>      > ##### 自动装配
>      >
>      > ​		通过**@Autowired**注解，修饰字段，或者成员方法，将声明的所需依赖，在bean实例化时，自动装配。
>      >
>

### 3. 单例性

> **Spring** 中的**Bean**都是单例的。Spring会拦截对**@Bean注解方法**的调用，确保返回的是**Spring**所创建的**Bean**，也就是Spring在调用**@Bean注解方法**时所创建的实例

## Controller

### 1.@RestController

被注解修饰的类是一个**RESTful**风格的**Controller**。主要用于**前后端分离项目**。返回纯数据。

### 2.@RequestMapping

这个注解修饰一个**控制器方法**，默认参数**value**为该方法的**路由路径**，**method**参数指明该方法的接受的**请求类型**。

### 3.@RequestParam

这个注解修饰**控制器方法的参数**，作用是**将该参数映射到前端请求中的一个具体字段**，默认参数值为请求中的字段名。

### 4.@RequestBody

这个注解修饰**控制器方法的参数**，作用是**将被修饰的参数与请求体中的json数据建立映射关系**。

### 5.@PathVariable

这个注解修饰**控制器方法的参数**，作用是**动态获取请求路径中的参数，使之映射到同名的方法参数。**

```java
@GetMapping("/user/{id}")
public String getUser( @PathVariable int id){
    return "根据ID获取用户";    
}
```



## Spring-Boot-Devtools

这是一个Spring Boot提供的**开发时**组件。

### 1)解决什么问题

在实际的项目开发调试过程中，会频繁地修改后台类文件，导致需要重新编译，重新启动，整个过程十分麻烦，影响开发效率。

### 2)基本实现

spring-boot-devtool会监听**classpath**下的文件变动，触发**Restart**类加载器重新加载该类，从而实现类文件和属性文件的热部署。

### 3)基本配置

```java
#热部署生效
spring.devtools.restart.enable=true
#设置重启目录
spring.devtools.restart.additional-paths=path/to/the/directory
#设置classpath下的WEB-INF目录下的修改不重启
spring.devtools.restart.exclude=static/**
```

