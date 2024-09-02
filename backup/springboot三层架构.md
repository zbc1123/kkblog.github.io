# 使用Spring Boot实现三层架构

在现代软件开发中，三层架构是一种常见的设计模式。它将应用程序分为三个主要部分：表示层（Controller）、业务逻辑层（Service）和数据访问层（Mapper/Repository）。这种分层结构有助于提高代码的可维护性和可扩展性。

本文将介绍如何使用Spring Boot实现三层架构。

## 项目结构

首先，我们来看一下项目的基本结构：

```

src
├── main
│   ├── java
│   │   └── com
│   │       └── example
│   │           ├── controller
│   │           ├── service
│   │           │   └── impl
│   │           ├── mapper
│   │           └── domain
│   └── resources
│       └── application.properties
└── test
````

## 表示层（Controller）

表示层负责处理HTTP请求，并将请求转发到业务逻辑层。以下是一个简单的Controller示例：

```java:src/main/java/com/example/controller/UserController.java
package com.example.controller;

import com.example.service.UserService;
import com.example.domain.User;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }
}
````


## 业务逻辑层（Service）

业务逻辑层包含应用程序的核心业务逻辑。以下是一个简单的Service接口和实现类示例：

```java:src/main/java/com/example/service/UserService.java
package com.example.service;

import com.example.domain.User;

import java.util.List;

public interface UserService {
    List<User> getAllUsers();
    User createUser(User user);
}
```

```java:src/main/java/com/example/service/impl/UserServiceImpl.java
package com.example.service.impl;

import com.example.domain.User;
import com.example.mapper.UserMapper;
import com.example.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserServiceImpl implements UserService {

    @Autowired
    private UserMapper userMapper;

    @Override
    public List<User> getAllUsers() {
        return userMapper.findAll();
    }

    @Override
    public User createUser(User user) {
        userMapper.insert(user);
        return user;
    }
}
```


## 数据访问层（Mapper）

数据访问层负责与数据库进行交互。以下是一个简单的Mapper示例：

```java:src/main/java/com/example/mapper/UserMapper.java
package com.example.mapper;

import com.example.domain.User;
import org.apache.ibatis.annotations.*;

import java.util.List;

@Mapper
public interface UserMapper {

    @Select("SELECT * FROM users")
    List<User> findAll();

    @Insert("INSERT INTO users(name, email) VALUES(#{name}, #{email})")
    @Options(useGeneratedKeys = true, keyProperty = "id")
    void insert(User user);
}
```


## 数据模型（Domain）

数据模型用于表示数据库中的数据。以下是一个简单的User模型示例：

```java:src/main/java/com/example/domain/User.java
package com.example.domain;

public class User {
    private Long id;
    private String name;
    private String email;

    // Getters and Setters
}
```


## 配置文件

在`application.properties`中配置数据库连接信息：

```properties:src/main/resources/application.properties
spring.datasource.url=jdbc:mysql://localhost:3306/your_database
spring.datasource.username=your_username
spring.datasource.password=your_password
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
mybatis.mapper-locations=classpath:mapper/*.xml
```


## 总结

通过将应用程序分为表示层、业务逻辑层和数据访问层，我们可以更好地组织代码，提高代码的可维护性和可扩展性。Spring Boot和MyBatis提供了强大的支持，使得实现三层架构变得非常简单。
