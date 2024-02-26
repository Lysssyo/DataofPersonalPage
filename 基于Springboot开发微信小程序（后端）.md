# 基于Springboot开发微信小程序（后端）

## 项目依赖

​		`MyBatis Framework`，`MySQL Driver`，`Lombok`以及，`Spring Web`

```xml
<!-- pom.xml-->
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.lysssyo</groupId>
    <artifactId>WXMiniProam</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>WXMiniProam</name>
    <description>WXMiniProam</description>
    <properties>
        <java.version>1.8</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <spring-boot.version>2.6.13</spring-boot.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>net.sourceforge.htmlunit</groupId>
            <artifactId>htmlunit</artifactId>
            <version>2.55.0</version>
        </dependency>
        <dependency>
            <groupId>org.jsoup</groupId>
            <artifactId>jsoup</artifactId>
            <version>1.8.3</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.2.2</version>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.1</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>${spring-boot.version}</version>
                <configuration>
                    <mainClass>com.lysssyo.WxMiniProamApplication</mainClass>
                </configuration>
                <executions>
                    <execution>
                        <id>repackage</id>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```



## 项目配置

```properties
#application.properties
# 应用服务 WEB 访问端口
server.port=80

#数据库连接
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://数据库IP/DatabaseForWX
spring.datasource.username=用户名
spring.datasource.password=密码

#开启mybatis的日志输出
mybatis.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl

#开启数据库表字段 到 实体类属性的驼峰映射
mybatis.configuration.map-underscore-to-camel-case=true
```



## 项目结构

![image-20240226163334163](WXMiniProAboveSpringboot2.assets/image-20240226163334163.png)

> WXCrawlingService是为了实现其他功能

## 代码实现

​		后端其实较为简单，只需要把前端请求的数据给前端就好了，不用做过多的数据处理：

DocController:

```java
@Slf4j
@RestController
public class DocController {
    @Autowired
    private DocService docService;
    
    @GetMapping("/all")
    public Result AllData(){
        log.info("查询所有数据");
        List<Doc> DocList=docService.AllData();
        return Result.success(DocList);
    }
    
    @GetMapping("/getPage/{style}")
    public Result PageData(@PathVariable Integer style){
        log.info("根据路径参数page查询对应的页面数据");
        List<Doc> DocList=docService.pageData(style);
        return Result.success(DocList);
    }
    
    @GetMapping("/getDoc/{id}")
    public Result getDocMain(@PathVariable Integer id){
        List<Doc> DocMain=docService.getDocMain(id);
        return Result.success(DocMain);
    }
}
```

DocServiceImpl:

```java
@Service
public class DocServiceImpl implements DocService {
    @Autowired
    private DocMapper docMapper;
    public List<Doc> AllData() {
        List<Doc> docList = docMapper.AllData();
        return docList;
    }
    @Override
    public List<Doc> pageData(Integer style) {
        List<Doc> docList = docMapper.pageData(style);
        return docList;
    }
    @Override
    public List<Doc> getDocMain(Integer id) {
        List<Doc> docList = docMapper.getDoc(id);
        return docList;
    }
}
```

mapper:

```java
@Mapper
public interface DocMapper {
    //查询表中所有数据
    @Select("select id,style,tittle,updateTime,createTime,DocMain from Doc")
    List<Doc> AllData();

    //根据参数style，查询页面数据
    @Select("select id,style,tittle,updateTime from Doc where style=#{style}")
    List<Doc> pageData(Integer style);

    //根据id，查询文档
    @Select("select id,style,tittle,updateTime,createTime,DocMain from Doc where id=#{id}")
    List<Doc> getDoc(Integer id);
}
```

pojo中的Doc:

```java
public class Doc {
    private short id;//文档的id
    private short style;//文档的类型
    private String tittle;//文档的标题
    private String createTime;//文档的上传时间
    private String updateTime;//文档的修改时间
    private String DocMain;//文档的内容
}
```



## 请求与响应

### 1. 进入页面时发起网络请求

1.1 基本信息

> 请求路径：/getPage
>
> 请求方式：GET
>
> 接口描述：该接口用于进入小程序页面时的文档预览



1.2 请求参数

参数格式：路径参数

参数说明：

| 参数名 | 类型   | 是否必须 | 备注     |
| ------ | ------ | -------- | -------- |
| style  | number | 必须     | 页面的ID |

请求参数样例：

```
/getPage/1
```



1.3响应数据

参数格式：application/json

参数说明：

| 参数名         | 类型      | 是否必须 | 备注                           |
| -------------- | --------- | -------- | ------------------------------ |
| code           | number    | 必须     | 响应码，1 代表成功，0 代表失败 |
| msg            | string    | 非必须   | 提示信息                       |
| data           | object[ ] | 必须     | 返回的数据                     |
| \|-id          | number    | 必须     | 文件对应的id                   |
| \|- tittle     | string    | 必须     | 文件名                         |
| \|- updateTime | string    | 必须     | 修改时间                       |



### 2. 跳转markdown页面时发起网络请求

2.1 基本信息

> 请求路径：/getDoc
>
> 请求方式：GET
>
> 接口描述：该接口用于获取markdown文件



2.2 请求参数

参数格式：路径参数

参数说明：

| 参数名 | 类型   | 是否必须 | 备注     |
| ------ | ------ | -------- | -------- |
| id     | number | 必须     | 文档的id |

请求参数样例：

```
/getDoc/1
```



2.3响应数据

参数格式：application/json

参数说明：

| 参数名     | 类型      | 是否必须 | 备注                           |
| ---------- | --------- | -------- | ------------------------------ |
| code       | number    | 必须     | 响应码，1 代表成功，0 代表失败 |
| msg        | string    | 非必须   | 提示信息                       |
| data       | object[ ] | 必须     | 返回的数据                     |
| \|-docMain | string    | 必须     | markdown文件                   |



## 数据库

​		数据库中仅用存一张表：

![image-20240226164222727](WXMiniProAboveSpringboot2.assets/image-20240226164222727.png)

```sql
CREATE TABLE Doc
(
    id INT AUTO_INCREMENT PRIMARY KEY comment '文档的id',
    style INT NOT NULL comment '文档的类型，0：前端，1：后端，2：算法，3：我的',
    title VARCHAR(50) NOT NULL comment '文档的标题',
    updateTime DATE NOT NULL comment '修改时间',
    createTime DATE NOT NULL comment '上传时间',
    DocMain MEDIUMTEXT NOT NULL comment '文档的主要内容'
)
    COMMENT='微信小程序数据库表';
```

​	如果要上传新的文档，需要在数据库表中新增一行。



