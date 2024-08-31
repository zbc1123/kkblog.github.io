# Java中的数据实体类：POJO、DTO和更多

在Java开发中，我们经常需要处理和传输数据。为了更好地组织和管理这些数据，我们使用各种类型的数据实体类。本文将介绍几种常见的数据实体类，包括POJO、DTO等，并探讨它们的用途和区别。

## 1. POJO (Plain Old Java Object)

POJO是"简单老式Java对象"的缩写，它是最基本的Java类形式。

### 特点：
- 不继承任何类
- 不实现任何接口
- 不依赖于任何特定的框架

### 示例：
```java
public class Person {
    private String name;
    private int age;

    // Getters and setters
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}
```
## 2. DTO (Data Transfer Object)
DTO是用于在不同层或系统间传输数据的对象。

### 特点：
只包含数据，没有业务逻辑
通常用于网络传输或跨层数据交换
可以组合多个POJO的数据
### 示例：
```java
public class UserDTO {
    private String username;
    private String email;
    // 注意：不包含敏感信息如密码
    
    // Constructors, getters, and setters
}
```
## 3. Entity
Entity通常指的是与数据库表直接对应的类。

### 特点：
通常带有ORM（对象关系映射）注解
代表数据库中的一条记录
### 示例：
```java
@Entity
@Table(name = "users")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;
    
    @Column(name = "username")
    private String username;
    
    @Column(name = "email")
    private String email;
    
    // Getters and setters
}
```
## 4. VO (Value Object)
VO是用于展示层的对象，通常包含用于视图展示的数据。

### 特点：
可能组合多个实体的数据
针对特定视图或接口定制
### 示例：
```java
public class ProductVO {
    private String name;
    private BigDecimal price;
    private String category;
    private int stockQuantity;
    
    // Constructors, getters, and setters
}
```
总结
POJO：最基本的Java对象
DTO：用于数据传输的对象
Entity：与数据库表对应的对象
VO：用于视图展示的对象