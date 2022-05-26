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
  
> jdk8 的pom.xml

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

> jdk11的pom.xml

```xml

<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <parent>
        <artifactId>wangxiao-manage-platform</artifactId>
        <groupId>com.wangxiao</groupId>
        <version>1.0-SNAPSHOT</version>
    </parent>
    <modelVersion>4.0.0</modelVersion>

    <artifactId>wangxiao-system-base</artifactId>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <maven.compiler.version>3.8.1</maven.compiler.version>
        <maven.compiler.encoding>UTF-8</maven.compiler.encoding>
        <spring-boot.version>2.3.2.RELEASE</spring-boot.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <org.mapstruct.version>1.4.2.Final</org.mapstruct.version>
        <lombok.version>1.18.20</lombok.version>
    </properties>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.20</version>
            <scope>provided</scope>
        </dependency>

    </dependencies>
    <dependencyManagement>
        <dependencies>
        <!-- springboot 声明式依赖，并不实际依赖，用于管理版本号 -->
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
                <version>${maven.compiler.version}</version>
                <configuration>
                    <source>11</source>
                    <target>11</target>
                    <annotationProcessorPaths>
                        <path>
                            <groupId>org.mapstruct</groupId>
                            <artifactId>mapstruct-processor</artifactId>
                            <version>${org.mapstruct.version}</version>
                        </path>

                        <path>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                            <version>${lombok.version}</version>
                        </path>
                        <path>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok-mapstruct-binding</artifactId>
                            <!-- 如果是0.1.0 有可能出现生成了maptruct的实现类，但该类只创建了对象，没有进行赋值 -->
                            <version>0.2.0</version>
                        </path>

                    </annotationProcessorPaths>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.5.4</version>
                <configuration>
                    <!-- 指定该Main Class为全局的唯一入口 -->
                    <mainClass>com.wangxiao.base.SystemBaseApplication</mainClass>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <!--可以把依赖的包都打包到生成的Jar包中-->
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>com.spotify</groupId>
                <artifactId>docker-maven-plugin</artifactId>
                <version>1.2.2</version>
                <configuration>
                    <!-- 镜像名称  -->
<!--                    <imageName>172.16.1.124:8082/${project.artifactId}</imageName>-->
                    <imageName>172.16.1.235:8082/${project.artifactId}</imageName>
                    <dockerDirectory>${project.basedir}/src/main/docker</dockerDirectory>
                    <!-- docker远程服务地址 ，前提是docker服务器需开启远程访问-->
<!--                    <dockerHost>http://172.16.1.124:2375</dockerHost>-->
                    <dockerHost>http://172.16.1.235:2375</dockerHost>
                    <resources>
                        <resource>
                            <targetPath>/</targetPath>
                            <!-- 资源所在目录 -->
                            <directory>${project.build.directory}</directory>
                            <!-- 生成的.jar文件 -->
                            <include>${project.build.finalName}.jar</include>
                        </resource>
                        <resource>
                            <targetPath>/</targetPath>
                            <!-- 资源所在目录 -->
                            <directory>${project.build.outputDirectory}</directory>
                        </resource>
                    </resources>
                    <serverId>zhongda-docker</serverId>
                    <forceTags>true</forceTags>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>

```


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

    //public static final ResourceTagMapStructMapper INSTANCE = Mappers.getMapper(ResourceTagMapStructMapper.class);

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

## 七，我的封装使用经验

+ 基础接口封装

```java

import org.apache.commons.lang3.StringUtils;
import org.springframework.beans.BeanUtils;
import org.springframework.beans.BeansException;

import java.lang.reflect.InvocationTargetException;
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;
import java.util.function.Supplier;
import java.util.stream.Collectors;

/**
 * 类型转换接口，定义了默认的通用方法
 *
 * @author osc
 * @createTime 2021-12-07 14:18
 */
public interface BaseMapStructMapper {

    /**
     * <p> List<T>集合中的类型转换，适应于集合中的类属性名称一样的转换 <p/>
     *
     * @param clazz      返回的类型对象
     * @param sourceList 转换前的List集合，k泛型参数
     * @return 返回转换后的List集合
     */
    default <K, T> List<T> toTargetList(List<K> sourceList, Class<T> clazz) {

        if (sourceList != null && !sourceList.isEmpty()) {

            return sourceList.stream().map(e -> {
                T target = newClass(clazz);
                BeanUtils.copyProperties(e, target);
                return target;
            }).collect(Collectors.toList());
        }

        return Collections.emptyList();
    }


    /**
     * 属性名称有就一致的对象 之间转换 方法
     *
     * @param k     泛型参数
     * @param clazz 返回的类对象
     * @return 返回转换后的类型对象实例
     */

    default <K, T> T toTargetObj(K k, Class<T> clazz) {

        try {
            if (k != null) {
                T target = newClass(clazz);
                if (target != null) {
                    BeanUtils.copyProperties(k, target);
                    return target;
                }
            }
        } catch (BeansException e) {
            e.printStackTrace();
        }
        return null;
    }


    /**
     * List集合数据的拷贝
     *
     * @param sources: 数据源类
     * @param target:  目标类::new(eg: UserVO::new)
     * @return 集合对象
     */
    default <S, T> List<T> copyListProperties(List<S> sources, Supplier<T> target) {

        return copyListProperties(sources, target, null);
    }


    /**
     * 带回调函数的list集合数据的拷贝（可自定义字段拷贝规则）
     *
     * @param sources:  数据源类
     * @param target:   目标类::new(eg: UserVO::new)
     * @param callBack: 回调函数  //BeanUtilCopyCallBack 自定义函数式接口
     *                  List<UserVO> userVOList = obj.copyListProperties(userDOList, UserVO::new, (userDO, userVO) -> {
     *                  // 这里可以定义特定的转换规则
     *                  userVO.setSex(SexEnum.getDescByCode(userDO.getSex()).getDesc());
     *                  });
     * @return 集合对象
     */
    default <S, T> List<T> copyListProperties(List<S> sources, Supplier<T> target, BeanUtilCopyCallBack<S, T> callBack) {
        List<T> list = new ArrayList<>(sources.size());

        for (S source : sources) {
            T t = target.get();
            BeanUtils.copyProperties(source, t);
            list.add(t);
            if (callBack != null) {
                callBack.callBack(source, t);
            }
        }
        return list;
    }

    /**
     * 字符串集合转换成 long集合
     *
     * @param resourceIds 字符串集合
     * @return long集合
     */
    default List<Long> toLongList(List<String> resourceIds) {

        List<Long> longList = new ArrayList<>(16);
        if (!resourceIds.isEmpty()) {
            longList = resourceIds.stream().filter(StringUtils::isNotBlank).map(
                    Long::parseLong
            ).collect(Collectors.toList());
        }

        return longList;
    }

    /**
     * 根据泛型创建对象
     *
     * @param clazz 类对象
     * @return 返回 对应类的实例
     */
    private static <T> T newClass(Class<T> clazz) {

        //jdk11 推荐
        T target = null;
        try {
            target = clazz.getDeclaredConstructor().newInstance();
        } catch (InstantiationException | IllegalAccessException | NoSuchMethodException | InvocationTargetException e) {
            e.printStackTrace();
        }
        return target;

    }
}

```

+ 使用参考类

```java

package com.wangxiao.marketing.application.assembler;

import com.wangxiao.marketing.application.query.dto.ProductAppDTO;
import com.wangxiao.marketing.application.query.dto.SubjectProductAppDTO;
import com.wangxiao.marketing.infrastructure.db.dataobject.ProjectSubjectTreeDO;
import com.wangxiao.marketing.infrastructure.dto.ProductListDTO;
import com.wangxiao.marketing.preference.enums.MarketingEnums;
import com.wangxiao.marketing.preference.utils.BaseMapStructMapper;
import org.mapstruct.Mapper;
import org.mapstruct.Mapping;

import java.util.List;

/**
 * 通用功能应用层 类型转换类；
 * componentModel = "spring" 让此类注入了spring容器中
 *
 * @author osc
 * @create 2022-03-24 17:03
 */

@Mapper(componentModel = "spring", imports = {MarketingEnums.class})
//uses = {StringMapperA.class} 引入spring 容器中的对象
//@Mapper(componentModel = "spring", uses = {StringMapperA.class})
public abstract class CommonManageAppAssembler implements BaseMapStructMapper {


//    也可以这样注入
//    @Autowired
//    protected StringMapperB stringMapperB;

    /**
     * 给加上默认值 proTypeId
     *
     * @param product 参数
     * @return 转换后的值
     */
     
     // 也可以这样使用
     //@Mapping(target = "personList", expression = "java(stringMapperB.xyz(product.getPersonList()))")
    @Mapping(target = "proTypeName", expression = "java(getMessageByCode(product.getProductType()))")
    public abstract ProductAppDTO toProductAppDTO(ProductListDTO product);


    /**
     * 集合转换
     *
     * @param productList 集合对象
     * @return 转换后集合
     */
    public abstract List<ProductAppDTO> toProductAppList(List<ProductListDTO> productList);

    /**
     * 转换成科目对象
     *
     * @param projectSubjectTree 参数
     * @return 科目对象
     */
    @Mapping(target = "subjectId", source = "id")
    @Mapping(target = "subjectName", source = "name")
    public abstract SubjectProductAppDTO toSubjectProductAppDTO(ProjectSubjectTreeDO projectSubjectTree);

    /**
     * 转换成科目对象集合
     *
     * @param projectSubjectTreeList 参数
     * @return 科目对象集合
     */
    public abstract List<SubjectProductAppDTO> toSubjectProductAppList(List<ProjectSubjectTreeDO> projectSubjectTreeList);


    /**
     * 根据数字获取相应的名称
     *
     * @param code 数据号
     * @return 名称
     */
    static String getMessageByCode(Integer code) {

        String message = "";

        if (code == null) {
            return null;
        }
        switch (code) {
            case 1:
                message = MarketingEnums.PRE_TYPE.QUE.getMessage();
                break;

            case 2:
                message = MarketingEnums.PRE_TYPE.COURSE.getMessage();
                break;

            case 3:
                message = MarketingEnums.PRE_TYPE.MATERIAL.getMessage();
                break;
            default:
                message = "未知类型";
        }
        return message;
    }

}


// spring 容器中的对象
@Component
public static class StringMapperB {
    
    public List<PersonDto> xyz(List<Person> list) {
        return list.stream().map(p -> new PersonDto(p.getBoy())).collect(Collectors.toList());
    }
}

```