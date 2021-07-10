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

+ 方案一
  
~~~xml

<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
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
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
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

    @Mapping(target = "orderStatus", expression = "java(DictUtils.getDictLabel(\"order_status\",orderExport.getOrderStatus().toString(),null))") //使用工具类处理
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

+ [mapstruct 高级用法自定义转换规则-参考案例](https://blog.csdn.net/sunboylife/article/details/115706803?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~aggregatepage~first_rank_v2~rank_aggregation-1-115706803.pc_agg_rank_aggregation&utm_term=mapstruct+%E8%87%AA%E5%AE%9A%E4%B9%89%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2&spm=1000.2123.3001.4430)
+ [mapstruct+lombok+validator，简化代码三剑客-参考案例](https://blog.csdn.net/MingLiang000/article/details/82726571?utm_medium=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~default-18.base&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~BlogCommendFromBaidu~default-18.base)