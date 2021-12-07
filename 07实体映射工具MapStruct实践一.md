# MapStruct

## 一，官方文档
+ [MapStruct官网地址](https://mapstruct.org/)
+ [MapStruct 1.4.2.Final文档](https://mapstruct.org/documentation/stable/reference/html/)
+ [MapStruct 1.3.0.Final参考指南-中文版](http://www.kailing.pub/MapStruct1.3/index.html#_apache_maven)
+ [5种常见Bean映射工具的性能比对](https://www.cnblogs.com/javaguide/p/11861749.html)

## 二，MapStruct - Lombok & Maven 版本关系

+ Maven版本要3.6.0以上
+ Lombok版本要1.16.16以上
+ （仅针对方案一）另外编译的 Lombok、Mapstruct 的插件在pom.xml文件中的plugin不要忘记加上，否则会引发如下报错：

>No property named “XXX“ exists in source parameter(s). Did you mean “null“?

### pom.xml文件中引入：

+ 方案一: jdk1.8 以上能使用，jdk1.8和jdk11已经验证过了，指定对应的jdk版本即可，其他不变。
  
~~~xml

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!-- <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target> -->
    <!-- 版本1.4.0.Final 和 1.4.1.Final 都可以 -->
    <org.mapstruct.version>1.4.1.Final</org.mapstruct.version>
    <org.projectlombok.version>1.18.12</org.projectlombok.version>
</properties>
 
<dependencies>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct</artifactId>
        <version>${org.mapstruct.version}</version>
    </dependency>
 
    <!-- lombok dependencies should not end up on classpath -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${org.projectlombok.version}</version>
        <scope>provided</scope>
    </dependency>
 
    <!-- idea 2018.1.1 之前的版本需要添加下面的配置，后期的版本就不需要了，可以注释掉。 -->
    <dependency>
          <groupId>org.mapstruct</groupId>
          <artifactId>mapstruct-processor</artifactId>
          <version>${org.mapstruct.version}</version>
          <scope>provided</scope>
    </dependency>
</dependencies>
 
<build>
    <plugins>
    <!-- maven编译 设置-->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
            <!-- 选择maven编译时jdk的版本 1.8 或者 11-->
                <source>1.8</source>
                <target>1.8</target>
                <annotationProcessorPaths>
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok</artifactId>
                        <version>${org.projectlombok.version}</version>
                    </path>
                    <path>
                        <groupId>org.mapstruct</groupId>
                        <artifactId>mapstruct-processor</artifactId>
                        <version>${org.mapstruct.version}</version>
                    </path>
                      <!-- additional annotation processor required as of Lombok 1.18.16 -->
                      <!-- 如果lombok 的版本是1.18.16 及以上版本时， 需要引入这个编译依赖包 -->
                    <path>
                        <groupId>org.projectlombok</groupId>
                        <artifactId>lombok-mapstruct-binding</artifactId>
                        <!-- 版本0.1.0 有可能会出现生成了mapstruct实现类，但是该类创建了对象，没有赋值的情况   -->
                        <version>0.2.0</version>
                    </path>
                </annotationProcessorPaths>
            </configuration>
        </plugin>
    </plugins>
</build>

~~~

+ 方案二
  
```xml

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <!--  <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target> -->
    <org.mapstruct.version>1.4.1.Final</org.mapstruct.version>
    <org.projectlombok.version>1.18.12</org.projectlombok.version>
</properties>
 
<dependencies>
    <dependency>
        <groupId>org.mapstruct</groupId>
        <artifactId>mapstruct-jdk8</artifactId>
        <version>${org.mapstruct.version}</version>
    </dependency>
    <dependency>
          <groupId>org.mapstruct</groupId>
          <artifactId>mapstruct-processor</artifactId>
          <version>${org.mapstruct.version}</version>
          <scope>provided</scope>
    </dependency>
 
    <!-- lombok  -->
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${org.projectlombok.version}</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
 
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.8.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>

```

## 三，mapstruct 高级用法自定义转换规则

+ 注意：使用表达式的时候，类必须是全路径的使用，或者@Mapper(imports={类名.class}，使用表达式进行定义类型转换，expression="java(导入类的方法掉用)"
+ componentModel = "spring" ，将此接口生成的实现类加入到spring的 IOC 容器中

~~~java

import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.factory.Mappers;

import java.util.List;

/**
 * @author osc
 * @create 2021-07-10 10:41
 */
@Mapper(componentModel = "spring",imports = {DictUtils.class})
public interface OrderExportMapper {

    OrderExportMapper INSTANCE = Mappers.getMapper(OrderExportMapper.class);

   // expression 表达式可以使用工具类处理，或者是导入的类中方法处理。此处为导入的工具类处理
    @Mapping(target = "orderStatus", expression = "java(DictUtils.getDictLabel(\"order_status\",orderExport.getOrderStatus().toString(),null))")
    OrderExportRes transToOrderExportRes(OrderExport orderExport);

    List<OrderExportRes> orderExportsToOrderExportResEs(List<OrderExport> orderExportList);
    
}

~~~

## 四，配置好接口后的使用

~~~java
// 在需要使用的地方，注入，然后调用其方法实现对象之间的映射
@Autowired
private OrderExportMapper orderExportMapper;

List<OrderExportRes> orderExportResEs = orderExportMapper.orderExportsToOrderExportResEs(ordersExport);

~~~

## 五，使用案例参考

+ [mapstruct 高级用法自定义转换规则-参考案例](https://blog.csdn.net/sunboylife/article/details/115706803)
+ [mapstruct+lombok+validator，简化代码三剑客-参考案例](https://blog.csdn.net/MingLiang000/article/details/82726571)

## 六， Mapstruct - 如何在Generated Mapper类中注入spring依赖项

+ 如果需要在生成的mapper实现中注入一个spring服务类,以便我可以通过表达式来使用它
  
  `@Mapping(target="x", expression="java(myservice.findById(id))")"`

+ 我发现这一点的唯一方法是:

```text
    1、将我的mapper接口转换为抽象类
    2、在抽象类中注入服务
    3、注入的服务使用protected修饰,以便抽象类的"实现"具有访问权限
```

+ 正在使用的CDI,但它应该与Spring相同

```java
@Mapper(
unmappedTargetPolicy = org.mapstruct.ReportingPolicy.IGNORE,
componentModel = "spring",
uses = {
// My other mappers...
})
public abstract class ResourceTagMapStructMapper {

    public static final ResourceTagMapStructMapper INSTANCE = Mappers.getMapper(ResourceTagMapStructMapper.class);

    @Autowired
    protected TagCacheRepository tagCacheRepository;

    /**
     * 对象转换
     * @param db
     * @return
     */
    @Mapping(target = "associatedResourcesNum",
            expression = "java( tagCacheRepository.getResourceCountByTypeIdTagId(db.getResourceTypeId(), db.getResourceTagId()).intValue())")
    public abstract ResourceTagListAppDto dOtoResourceTagListAppDto(ResourceTagDO db);


    /**
     * 转换page 对象，此处是 Mybatis-plus 中的Page类型
     * @param tagDOPage
     * @return
     */
    public abstract Page<ResourceTagListAppDto> toPageDTO(Page<ResourceTagDO> tagDOPage);

}

```

+ 定义好的抽象类 ResourceTagMapStructMapper 如何使用：

```java
@Service
public class ResourceTagQueryServiceImpl implements ResourceTagQueryService {

    @Autowired
    private ResourceTagRepository resourceTagRepository;

    @Autowired
    private ResourceTagMapStructMapper resourceTagMapStructMapper;


    @Override
    public Page<ResourceTagListAppDto> tagList(Page requestPageParam, ResourceTagListAppVO conditionVo) {

        Page<ResourceTagDO> tagDOPage = resourceTagRepository.tagPage(requestPageParam, conditionVo);

        // 在此处调用，实现数据类型的转换
        return resourceTagMapStructMapper.toPageDTO(tagDOPage);
    }
}

```

+ [Mapstruct - 如何在Generated Mapper类中注入spring依赖项的博客参考链接](https://app.yinxiang.com/fx/77938c8e-291d-4f5f-b970-ec71a5f33f90)

## 在低于jdk8版本中mapstruct的使用--待验证

1、在pom.xml 文件中

~~~xml

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
        <version>2.0.4.RELEASE</version>
        <relativePath/>
    </parent>

    <dependencies>

   <!--模板对象与实体对象转换-->
        <dependency>
            <groupId>org.mapstruct</groupId>
            <!-- jdk8以下就使用mapstruct -->
            <artifactId>mapstruct-jdk8</artifactId>
            <version>1.2.0.Final</version>
        </dependency>
        <dependency>
            <groupId>org.mapstruct</groupId>
            <artifactId>mapstruct-processor</artifactId>
            <version>1.2.0.Final</version>
        </dependency>

        <!-- 添加lombok依赖 -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.16.20</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>spring_boot</finalName>
        <!-- 添加Spring boot的maven插件,可以不写版本号,在spring-boot-starter-parent已经包含  -->
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

~~~

2、在低于jdk8版本中mapstruct的使用的例子

+ [对应的参考例子githup地址](https://github.com/mmzsblog/mapstructDemo)