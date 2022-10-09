---
title: Springboot集成Dubbo2.7.8
categories:
- tech
---

# Springboot 集成Dubbo2.7.8示例

<!-- more -->

## consumer&provider

### 依赖

新建springboot项目后，引入以下额外依赖，Dubbo的consumer和provider的依赖相同

```xml
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>2.7.8</version>
</dependency>

<dependency>
    <groupId>com.101tec</groupId>
    <artifactId>zkclient</artifactId>
    <version>0.10</version>
</dependency>

<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>4.2.0</version>
</dependency>
```

### provider

建立子工程provider来作为生产者

![image-20210913215341110](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210913215341110.png)

其中application.properties:

```properties
server.port=8081
# 服务应用名字
dubbo.application.name=provider-server
# 注册中心地址
dubbo.registry.protocol=zookeeper
dubbo.registry.address=zookeeper://localhost:2181
# 哪些服务要被注册
dubbo.scan.base-packages=com.example.provider.service
# 协议
#dubbo.protocol.host=192.168.0.1
spring.main.allow-bean-definition-overriding=true
dubbo.protocol.name=dubbo
dubbo.protocol.port=20880
```

TicketServiceImpl:

版本号一定要填写！

```java
package com.example.provider.service;

import org.apache.dubbo.config.annotation.DubboService;

@DubboService(version = "1.0")
public class TicketServiceImpl implements TicketService{

    @Override
    public String getTicket(String name) {
        return name;
    }
}
```

TicketService：

```java
package com.example.provider.service;

public interface TicketService {

    String getTicket(String name);
}
```

### consumer

引入provider的接口，注意接口包路径一定要一致

目录：

![image-20210913215745601](C:\Users\Jet\AppData\Roaming\Typora\typora-user-images\image-20210913215745601.png)

application.properties:

```properties
server.port=8083

# 消费者也要暴露自己的名字
dubbo.application.name=consumer-server
# 注册中心的地址
dubbo.registry.protocol=zookeeper
dubbo.registry.address=zookeeper://localhost:2181
dubbo.scan.base-packages=com.example.consumer.service
# 协议
spring.main.allow-bean-definition-overriding=true
dubbo.protocol.name=dubbo
dubbo.protocol.port=20881
```

UserServiceImol:

@DubboService注解会让这个服务变成provider，而@DubboReference则让服务变成consumer，同时加的时候该服务既是provider又是consumer，版本号都要加，否则会报错

```java
package com.example.consumer.service;

import com.example.provider.service.TicketService;
import org.apache.dubbo.config.annotation.DubboReference;
import org.apache.dubbo.config.annotation.DubboService;
import org.springframework.stereotype.Component;

//@DubboService(version = "1.0")
@Component
public class UserServiceImpl implements UserServicei{
    //想拿到provider-server提供的票，要去注册中心拿到服务
    @DubboReference(version = "1.0") 
    TicketService ticketService;

    @Override
    public void buyTicket(){
        String ticket = ticketService.getTicket("我");
        System.out.println(ticket);
        System.out.println("shit");
    }
}
```

UerServicei：

```java
package com.example.consumer.service;

public interface UserServicei {
    public void buyTicket();
}
```

## 环境搭建

### 注册中心zookeeper

完成整个rpc框架调用还需要注册中心，这里使用zookeeper，并加上dubbo的监控中心来可视化监控服务

下载apache-zookeeper-3.7.0-bin

运行bin文件夹中的zkServer.cmd启动zookeeper

### 监控中心dubbo-admin

下载dubbo-admin

为前后端分离项目

启动其中的dubbo-admin-ui，这里使用vscode打开，在终端输入npm run dev

启动其中的dubbo-admin-server，这里用idea打开，直接运行即可
