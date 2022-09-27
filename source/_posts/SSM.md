---
title: SSM
categories:
- tech
---

# SSM

## Spring

### 简介

Spring是一个IOC(DI)和AOP容器框架。

可以管理所有的组件（类）

### IOC和AOP

> IOC控制反转（容器）

反转：将主动创建变为被动获取

> DI依赖注入

容器能知道某个类运行时需要另外哪个类，通过反射的形式，将容器中准备好的对象注入该类（利用反射给属性赋值）

#### bean

```xml
<!-- 无参创建对象（默认）-->
<!--        一个Bean标签可以注册一个组件（对象、类）
  			class要写注册的组件的全类名，
    		id是该对象的唯一标识
    		name是别名，可以同时取多个,以逗号或分号分隔
    -->
<bean id="person" class="com.demo.spring.person" name="li;zhang;wang">
<!--        使用property为Person对象的属性赋值-->
        <property name="lastName" value="张三"></property>
        <property name="age" value="23"></property>
        <property name="email" value="bni@bni.com"></property>
        <property name="gender" value="男"></property>
</bean>
    
<!-- 有参创建对象的三种方式-->
1.下标赋值
    <constructor-arg index="0" value="张三">
2.类型
    <constructor-arg type="java.lang.String" value="张三">
3.参数名
    <constructor-arg name="name" value="张三">
```

#### spring配置

1.别名alias

```xml
<!--    如果添加了别名，也可以使用别名-->
    <alias name="person" alias="li"></alias>
```

2.import

一般团队开发，可以导入其他的xml配置文件

```xml
<import resource="beans2.xml"/>
```



#### bean的作用域

1.单例模式（singleton）

全局只有一个对象

2.原型模式（prototype）

每次获取都是新的对象

#### bean自动装配

```java
<!--    byName会自动装配和set方法后的值相同的id的对象-->
<!--    byType会自动装配和set的类型相同的对象-->
    <bean id="person" class="com.demo.spring.Person" autowire="byName">
        
<!-- 使用注解自动装配，需要导入约束配置支持-->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">

    <context:annotation-config/>

</beans>
```

#### 注解

> @Component

类上加@Component相当于<bean id="person" class="com.demo.spring.person">

> 衍生注解

- dao [@Repository]
- service [@Service]
- controller [@controller]

#### 代理

> 静态代理

类似中介

> 动态代理

利用反射

- 一个动态代理类代理的是一个接口，一般就是对应的一类业务
- 一个动态代理类可以代理多个类，只要实现了同一个接口即可

```java
public class ProxyInvocationHandler implements InvocationHandler {

    private Object target;

    public Object getProxy(){
        return Proxy.newProxyInstance(this.getClass().getClassLoader(), target.getClass().getInterfaces(),this);
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        Object result = method.invoke(target, args);
        return result;
    }

    public void setTarget(Object target) {
        this.target = target;
    }

    public void log(String msg){
        System.out.println("执行了" + msg + "方法");
    }
}
```

#### Spring代理

## MyBatis

1.配置mybatis-config.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?useSSL=true&amp;useUnicode=true&amp;characterEncoding=UTF-8"/>
                <property name="username" value="root"/>
                <property name="password" value="1254503008"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

2.写配置类

```java
//工具类sqlSessionFactory --> sqlSession
public class MybatisUtils {

    static {
        String resource = "mybatis-config.xml";
        InputStream inputStream = null;
        try {
            inputStream = Resources.getResourceAsStream(resource);
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

}
```

### 万能map

假设实体类或库表中字段或参数过多，应当考虑使用map

### mybatis配置

- configuration（配置）
  - [properties（属性）](https://mybatis.org/mybatis-3/zh/configuration.html#properties)
  - [settings（设置）](https://mybatis.org/mybatis-3/zh/configuration.html#settings)
  - [typeAliases（类型别名）](https://mybatis.org/mybatis-3/zh/configuration.html#typeAliases)
  - [typeHandlers（类型处理器）](https://mybatis.org/mybatis-3/zh/configuration.html#typeHandlers)
  - [objectFactory（对象工厂）](https://mybatis.org/mybatis-3/zh/configuration.html#objectFactory)
  - [plugins（插件）](https://mybatis.org/mybatis-3/zh/configuration.html#plugins)
  - environments（环境配置）
    - environment（环境变量）
      - transactionManager（事务管理器）
      - dataSource（数据源）
  - [databaseIdProvider（数据库厂商标识）](https://mybatis.org/mybatis-3/zh/configuration.html#databaseIdProvider)
  - [mappers（映射器）](https://mybatis.org/mybatis-3/zh/configuration.html#mappers)

> typeAliases

可以给类起别名，也可以直接扫描一个包，其下所有类默认为小写名称、

```xml
<typeAliases>
    <typeAlias type="com.demo.pojo.User" alias="User"/>
    <package name="com.demo.pojo"/>
</typeAliases>
```

> 生命周期和作用域

![image-20210807111225663](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210807111225663.png)

### LOMBOK插件

自动编写getter，setter等，无需手动赋值

![image-20210809155458897](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210809155458897.png)

# springMVC

spingmvc框架以请求为驱动，围绕一个中心Servlet分派请求及提供其他功能。

![image-20210828233325754](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210828233325754.png)

使用DispatcherServlet来进行调度管理。



![image-20210827165707752](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210827165707752.png)

![image-20210827165750515](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210827165750515.png)

![image-20210827165755615](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210827165755615.png)

创建springmvc项目步骤

1. 新建一个web项目
2. 导入相关jar包
3. 编写web.xml，注册DispatcherServlet
4. 编写springmvc配置文件
5. 接下来创建对应的控制类，controller
6. 最后完善前端视图和controller之间的对应
7. 测试运行调试

springmvc必须配置三大件：

**处理器映射器，处理器适配器，视图解析器**

只需要手动配置视图解析器，而处理器映射器和处理器适配器只需要开启注解驱动即可

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/mvc
       http://www.springframework.org/schema/mvc/spring-mvc-4.2.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-4.2.xsd">

<!--    自动扫描包-->
    <context:component-scan base-package="com.demo.controller"/>
<!--    自动过滤静态资源，去除.mp4等处理不了的资源-->
    <mvc:default-servlet-handler />
    <!--支持mvc注解驱动
        在spring中一般采用@RequestMapping注解来完成映射关系
        要想使用@RequestMapping注解生效
        必须向上下文中注册DefaultAnnotationHandlerMapping
        和一个AnnotationMethodHanlerAdapter实例
        这两个实例分别在类级别和方法级别处理，
        而annotation-driven配置帮助我们自动完成上述两个实例的注入-->
    <mvc:annotation-driven/>

    <!--视图解析器 : 模板引擎 Thymeleaf Freemarker等-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

</beans>
```

# springboot

## 注解

```java
package cn.gacl.annotation;
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
/**
 * 这是一个自定义的注解(Annotation)类 在定义注解(Annotation)类时使用了另一个注解类Retention
 * 在注解类上使用另一个注解类，那么被使用的注解类就称为元注解
 * 
 * @author 
 * 
 */
@Target( { ElementType.METHOD, ElementType.TYPE })
//Target注解决定MyAnnotation注解可以加在哪些成分上，如加在类身上，或者属性身上，或者方法身上等成分
@Retention(RetentionPolicy.RUNTIME)
//Retention注解决定MyAnnotation注解的生命周期
/*
 * @Retention(RetentionPolicy.SOURCE)
 * 这个注解的意思是让MyAnnotation注解只在java源文件中存在，编译成.class文件后注解就不存在了
 * @Retention(RetentionPolicy.CLASS)
 * 这个注解的意思是让MyAnnotation注解在java源文件(.java文件)中存在，编译成.class文件后注解也还存在，
 * 被MyAnnotation注解类标识的类被类加载器加载到内存中后MyAnnotation注解就不存在了
 */
/*
 * 这里是在注解类MyAnnotation上使用另一个注解类，这里的Retention称为元注解。
 * Retention注解括号中的"RetentionPolicy.RUNTIME"意思是让MyAnnotation这个注解的生命周期一直程序运行时都存在
 */
@Documented 
//注解表明这个注解应该被 javadoc工具记录
@Inherited
//表名该注解所在的类若为父类则可以被子类继承该父类的注解
public @interface MyAnnotation {
}
```

```java
@Deprecated:若某类或某方法加上该注解之后，表示此方法或类不再建议使用，调用时也会出现删除线，但并不代表不能用，只是说，不推荐使用，因为还有更好的方法可以调用。
```

