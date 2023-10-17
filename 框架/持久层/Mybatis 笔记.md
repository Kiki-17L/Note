# Mybatis

Mybatis 是一个**持久层(Persistence)**框架。

## 基本注解

### 1.@MapperScan

> 这个注解，指定Mapper的扫描包

* 修饰对象：启动类

* 默认值：映射器Mapper组件所在的包

### 2.@Mapper

> 声明一个映射器（Mapper）组件。

* 修饰对象：映射器接口

## CRUD

### 1.增

```@Insert("insert into user values( #{id},#{username},#{password})")```

### 2.删

```@Delete("delete from user where id = #{id} ")```

### 3.改

```@Update("update user set username= #{username}, password = #{password} where id = #{id}")```

### 4.查

```@Select("Select * from user")```



```#{arg}```这个字符串格式是从被修饰方法的参数列表拿到参数。

## Mybatis-Plus

**MP**（Mybatis-Plus）是一个Mybatis的增强工具，在Mybatis的基础上**只做增强不做改变**。



### 基本配置

通过继承**BaseMapper**接口来使用Mybatis-Plus



### 常用注解

#### 1.@TableName

在实体类名与表名不一致时，映射实体类与表名。

#### 2.@TableField

在实体类字段名与表中字段名不一致时，映射实体类属性与表中字段。

#### 3.@TableId

描述主键字段属性。

