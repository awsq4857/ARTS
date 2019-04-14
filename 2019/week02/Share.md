# SpringBoot+Mybatis+MySQL增删改查

## SpringBoot+Mybatis+MySQL简介

SpringBoot是用来简化Spring应用的初始搭建和开发工程的框架。使用SpringBoot，可以极大的简化Spring应用的开发。

MyBatis是一个基于Java的持久层框架，它支持定制化SQL、存储过程以及高级映射。

## 创建Demo

### 创建工程

#### 1. 网页，IDE导入

* 打开网页https://start.spring.io/。
* 选择打包工具、语言、SpringBoot版本、项目信息、依赖包。
* 在项目依赖包中，我们需要添加，Web、MyBatis、MySQL。
* 点击最下面的按钮Generate Project，下载工程
* 打开IDEA，导入下载后的工程

#### 2. IDE

打开IDEA，新建工程，选择SpringBoot项目，然后依次填入上面的项目信息，生成工程。

### 创建数据库

#### 1. 数据库sql

```
DROP TABLE IF EXISTS `user`;
CREATE TABLE `user` (
  `id` bigint(20) DEFAULT NULL COMMENT '用户Id',
  `name` varchar(64) DEFAULT NULL COMMENT '名称',
  `create` datetime DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `modified` datetime DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '修改时间'
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

#### 2. 配置数据库连接信息
 
```
# 数据库配置
spring.datasource.url=jdbc:mysql://localhost:3306/study_mybatis_db
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

### 配置mybatis

#### 1. 创建Model

```
package com.example.mybatis.entity;

import java.io.Serializable;
import java.util.Date;

public class User implements Serializable {
    private Long id;
    private String name;
    private Date create;
    private Date modified;
    // 其余忽略
}
```

#### 2. 创建mapper，增删改查方法定义

```
package com.example.mybatis.mapper;

import com.example.mybatis.entity.User;
import org.apache.ibatis.annotations.*;
import org.apache.ibatis.type.JdbcType;

import java.util.List;

public interface UserMapper {
    @Select("select * from user where id = #{id}")
    @Results({
            @Result(column = "create",property = "create",jdbcType= JdbcType.DATE),
            @Result(column = "modified",property = "modified",jdbcType=JdbcType.DATE)
    })
    User queryOne(Long id);

    @Options(keyProperty="id",keyColumn="id",useGeneratedKeys=true)
    @Insert("insert into user(name) values(#{name})")
    int insert(User user);

    @Update("update user set name = #{name} where id=#{id}")
    void update(User user);

    @Delete("delete from user where id=#{id}")
    void delete(Long id);

    @Select("select * from user where name = #{name}")
    @Results({
            @Result(column = "create",property = "create",jdbcType=JdbcType.DATE),
            @Result(column = "modified",property = "modified",jdbcType=JdbcType.DATE)
    })
    List<User> queryByParams(@Param("name")String name);
}
```

#### 3. 设置mapper关联

工程属性文件application.properties，配置别名

`mybatis.type-aliases-package=com.example.mybatis.entity`

新建配置类，设置mapper的映射地址

```
package com.example.mybatis.util;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan("com.example.mybatis.mapper")//mapper地址
public class MybatisConfig {
}
```

### 业务逻辑，RestController

通过上述设置，我们就可以在我们的业务代码中，使用MyBatis进行数据库的增删改查了。

```
package com.example.mybatis.controller;

import com.example.mybatis.entity.User;
import com.example.mybatis.mapper.UserMapper;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyBatisController {
    @Autowired
    UserMapper userMpper;

    @RequestMapping(value = "/getUser", method = RequestMethod.GET)
    public User getUser(@RequestParam("id") long id) {
        User user = userMpper.queryOne(id);
        return user;
    }

    @RequestMapping(value = "/addUser", method = RequestMethod.GET)
    public User getUser(@RequestParam("name") String name,
                        @RequestParam(value = "create", required = false, defaultValue = "XXX") String create) {
        User user = new User();
        user.setName(name);
        userMpper.insert(user);
        return user;
    }
}
```

## 注意问题

1. 主键，数据库设置id为自增主键
2. 参数，可选、必选；默认值`@RequestParam(value = "create", required = false, defaultValue = "XXX")`



