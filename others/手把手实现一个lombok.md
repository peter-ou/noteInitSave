# 手把手实现一个lombok



## 一、lombok原理 JSR269

**什么是JSR ？**

[JSR](https://jcp.org/en/jsr/overview)是Java Specification Requests的缩写，意思是Java 规范提案。是指向JCP(Java Community Process)提出新增一个标准化技术规范的正式请求。任何人都可以提交JSR，以向Java平台增添新的API和服务。

有超过300的JSR。一些更为明显的JSRs包括：

|                           的JSR＃                            |                          规格或技术                          |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| [1](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [实时规范为Java](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（RTSJ规范）1.0 |
| [3](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java管理扩展](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JMX）的1.0，1.1和1.2 [[ 2 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [5](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java API的XML处理](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAXP）1.0 |
| [8](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [OSGI的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)开放服务网关规范 |
| [9](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [次郎](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（联邦管理体系规范）1.0 |
|       [12](http://www.360doc.com/content/18/0916/08/2)       | [Java数据对象](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JDO的）1.0 |
|       [13](http://www.360doc.com/content/18/0916/08/3)       | 改进的BigDecimal（[Java平台，标准版＃java.math](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
|       [14](http://www.360doc.com/content/18/0916/08/4)       | 加入到Java编程语言（如J2SE 5.0的[泛型类型](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
|       [16](http://www.360doc.com/content/18/0916/08/6)       | [Java EE连接器架构](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JCA）的1.0 |
|       [19](http://www.360doc.com/content/18/0916/08/9)       | [企业JavaBeans](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（EJB）2.0 |
| [22](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [JAIN SLEE API规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSLEE）的1.0 |
|       [30](http://www.360doc.com/content/18/0916/08/0)       | [连接有限设备配置](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（CLDC）1.0 [的Java ME](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|       [31](http://www.360doc.com/content/18/0916/08/1)       | [用于XML绑定的Java体系结构](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAXB）的1.0 |
|       [32](http://www.360doc.com/content/18/0916/08/2)       | [JAIN SIP API规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSIP）的1.0，1.1和1.2的Java ME |
|       [36](http://www.360doc.com/content/18/0916/08/6)       | [连接设备配置](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（CDC）的1.0为Java ME |
|       [37](http://www.360doc.com/content/18/0916/08/7)       | [移动信息设备描述](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（MIDP）1.0为Java ME |
| [40](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java元数据接口](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JMI）1.0 |
| [41](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 一个简单的[断言基金](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（J2SE 1.4中） |
| [47](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [日志](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API规范（J2SE 1.4中） |
| [48](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [WBEM服务规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（J2SE 1.4中） |
|       [51](http://www.360doc.com/content/18/0916/08/1)       | [新的I / O API的Java平台](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（J2SE 1.4的）（妞妞） |
|       [52](http://www.360doc.com/content/18/0916/08/2)       | [JavaServer Pages标准标记库](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSTL）的1.0和1.1 [[ 3 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|       [53](http://www.360doc.com/content/18/0916/08/3)       | [的Java Servlet](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 2.3和[JavaServer页面](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSP）的1.2规格 |
|       [54](http://www.360doc.com/content/18/0916/08/4)       | [Java数据库连接](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JDBC）3.0 |
|       [56](http://www.360doc.com/content/18/0916/08/6)       | [Java网络启动协议](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)和API（JNLP的），1.0，1.5和6.0 [[ 4 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（[Java Web Start的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
|       [58](http://www.360doc.com/content/18/0916/08/8)       | [Java 2平台企业版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（J2EE）的1.3 |
|       [59](http://www.360doc.com/content/18/0916/08/9)       | [Java 2平台标准版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（J2SE）的1.4（梅林） |
| [63](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [用于XML处理的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAXP）1.1和1.2 [[ 5 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [68](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java平台Micro版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（Java ME）的1.0 |
| [73](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java数据挖掘](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API（JDM）1.0 |
| [75](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [J2ME平台的PDA可选包](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|       [80](http://www.360doc.com/content/18/0916/08/0)       | 的Java [的USB](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API |
|       [82](http://www.360doc.com/content/18/0916/08/2)       | [蓝牙的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|       [88](http://www.360doc.com/content/18/0916/08/8)       |                    Java EE的应用程序部署                     |
|       [93](http://www.360doc.com/content/18/0916/08/3)       | [用于XML注册的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAXR）1.0 |
|       [94](http://www.360doc.com/content/18/0916/08/4)       | [Java规则引擎API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [102](http://www.360doc.com/content/18/0916/08/02)      | [Java的文档对象模型](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JDOM的）1.0 |
|      [110](http://www.360doc.com/content/18/0916/08/10)      | Java API的[WSDL](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（WSDL4J）1.0 |
|      [112](http://www.360doc.com/content/18/0916/08/12)      | [Java EE连接器架构](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JCA）的1.5 |
|      [113](http://www.360doc.com/content/18/0916/08/13)      | [的Java Speech API的2](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSAPI2） |
|      [114](http://www.360doc.com/content/18/0916/08/14)      | [Java数据库连接](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JDBC）的RowSet实现 |
|      [116](http://www.360doc.com/content/18/0916/08/16)      | [的SIP Servlet API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 1.0 |
|      [118](http://www.360doc.com/content/18/0916/08/18)      | [移动信息设备描述](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（MIDP）2.0为Java ME |
|      [120](http://www.360doc.com/content/18/0916/08/20)      | [无线消息API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（WMA）的 |
|      [121](http://www.360doc.com/content/18/0916/08/21)      | [应用程序隔离API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [127](http://www.360doc.com/content/18/0916/08/27)      | [的JavaServer Faces](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSF）的1.0和1.1 [[ 6 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [133](http://www.360doc.com/content/18/0916/08/33)      | [Java内存模型](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)和主题规范修订 |
|      [135](http://www.360doc.com/content/18/0916/08/35)      | Java ME的[Java移动媒体API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（MMAPI）的 |
|      [139](http://www.360doc.com/content/18/0916/08/39)      | [有限连接设备配置](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（CLDC）1.1为Java ME |
|      [140](http://www.360doc.com/content/18/0916/08/40)      | [服务定位协议](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) “（SLP）的Java API |
|      [141](http://www.360doc.com/content/18/0916/08/41)      | [会话描述协议](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（SDP）的API为Java |
|      [151](http://www.360doc.com/content/18/0916/08/51)      | [Java 2平台企业版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（J2EE）的1.4 |
|      [152](http://www.360doc.com/content/18/0916/08/52)      | [JavaServer页面](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSP）的2.0 |
|      [153](http://www.360doc.com/content/18/0916/08/53)      | [企业JavaBeans](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（EJB）2.1 |
|      [154](http://www.360doc.com/content/18/0916/08/54)      | [的Java Servlet](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 2.4和2.5规格[[ 7 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [160](http://www.360doc.com/content/18/0916/08/60)      | [Java管理扩展](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JMX）的远程API 1.0 |
|      [166](http://www.360doc.com/content/18/0916/08/66)      | [并发](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)实用程序（J2SE 5.0中`的java.util.concurrent`，`java.util.concurrent.atomic`和`java.util.concurrent.locks`） |
|      [168](http://www.360doc.com/content/18/0916/08/68)      | [Portlet规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 1.0 |
|      [170](http://www.360doc.com/content/18/0916/08/70)      | [内容库的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JCR）的1.0 |
|      [172](http://www.360doc.com/content/18/0916/08/72)      | [Java ME的Web服务规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [173](http://www.360doc.com/content/18/0916/08/73)      | [使用StAX](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（XML的流式API） |
|      [175](http://www.360doc.com/content/18/0916/08/75)      | [一个Java编程语言的元数据工具](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [176](http://www.360doc.com/content/18/0916/08/76)      | [Java 2平台标准版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（J2SE）的5.0（虎） |
|      [177](http://www.360doc.com/content/18/0916/08/77)      | [J2ME](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（SATSA的[安全和信任服务API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
|      [179](http://www.360doc.com/content/18/0916/08/79)      | [位置API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)为Java ME 1.0 |
|      [180](http://www.360doc.com/content/18/0916/08/80)      | [会话发起协议（SIP）API为Java ME](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [181](http://www.360doc.com/content/18/0916/08/81)      | 用于Java平台的[Web服务](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)元数据 |
|      [184](http://www.360doc.com/content/18/0916/08/84)      | [移动3D图形API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)为Java ME 1.0和1.1 |
|      [185](http://www.360doc.com/content/18/0916/08/85)      | [无线行业Java技术](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JTWI的） |
|      [187](http://www.360doc.com/content/18/0916/08/87)      | [即时消息](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（[的Java ME](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)和[Java SE中](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
|      [198](http://www.360doc.com/content/18/0916/08/98)      | 一个标准扩展API [的集成开发环境](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [199](http://www.360doc.com/content/18/0916/08/99)      | [Java编译器](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API |
| [201](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 扩展[Java编程语言的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)枚举，自动装箱，[静态导入](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)循环和增强（J2SE 5.0的） |
| [202](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java类文件](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)规范更新 |
| [203](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 更多[新的I / O API的Java平台](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（NIO2） |
| [204](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | Unicode增补字符支持（增加了J2SE 5.0的支持[Unicode的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 3.1） |
| [205](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [无线消息API 2.0](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) “（WMA）2.0 |
| [206](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [用于XML处理的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAXP）1.3 |
| [208](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java业务集成](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JBI）的1.0 |
| [215](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |                    Java社区进程（JCP）2.6                    |
| [218](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [连接设备配置](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（CDC）的1.1为Java ME |
|      [220](http://www.360doc.com/content/18/0916/08/0)       | [企业JavaBeans](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（EJB）3.0 |
|      [221](http://www.360doc.com/content/18/0916/08/1)       | [Java数据库连接](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JDBC）4.0 |
|      [222](http://www.360doc.com/content/18/0916/08/2)       | [用于XML绑定](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAXB）的2.0 [Java体系结构](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [223](http://www.360doc.com/content/18/0916/08/3)       | Java SE 6中[Java平台的脚本](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [224](http://www.360doc.com/content/18/0916/08/4)       | [XML Web服务的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAX-WS的），继承[的JAX-RPC](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [225](http://www.360doc.com/content/18/0916/08/5)       | [的XQuery API为Java](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（XQJ的） |
|      [226](http://www.360doc.com/content/18/0916/08/6)       | [可调节2D矢量图形](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API [的Java ME](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [229](http://www.360doc.com/content/18/0916/08/9)       | [支付API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（PAPI的） |
| [231](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [针对OpenGL的Java绑定](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [234](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [高级多媒体补充](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API为Java ME |
| [235](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [服务数据对象](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（SDO）， |
| [239](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [OpenGL](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) ES的Java绑定 |
| [240](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [JAIN SLEE API规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSLEE）的1.1 |
| [241](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Groovy编程语言](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [243](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java数据对象](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JDO的）2.0 |
| [244](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [的Java平台企业版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（Java EE）的5 |
| [880](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [JavaServer页面](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSP）的2.1 |
| [247](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java数据挖掘](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API（JDM）2.0 |
| [248](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |                         移动服务架构                         |
| [249](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |                        移动服务架构2                         |
| [250](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 常见的[注解](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)的Java平台（[Java元数据设施](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
| [252](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [的JavaServer Faces](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSF）的1.2 |
| [253](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [移动电话服务API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（MTA）， |
| [255](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java管理扩展](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JMX）2.0 |
| [256](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [移动传感器API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [257](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [非接触式通信API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（[NFC技术](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
| [260](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Javadoc的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)标签技术更新 |
| [269](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 可插拔[注解](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)处理API（[Java元数据设施](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
| [270](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java平台标准版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（Java SE）的6（野马） |
| [271](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [移动信息设备描述](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（MIDP）3.0为Java ME |
| [274](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [BeanShell的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)脚本语言 |
| [275](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 单位规范（见[计量单位](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
| [276](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 设计时[元数据](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)的[的JavaServer面临的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)组件 |
| [277](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java模块系统](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [280](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [对于Java ME的XML API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [281](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [IMS的服务API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（见[的IMS](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
| [282](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [为Java实时规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（RTSJ规范）1.1 |
| [283](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [内容库的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JCR）的2.0 |
| [286](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Portlet规范](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 2.0 |
| [289](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [的SIP Servlet API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 1.1 |
| [290](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Java语言与XML用户界面标记集成](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（XML用户界面） |
| [291](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | 针对Java SE动态组件的支持（见[的OSGi](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
| [292](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [JavaTM平台上支持动态类型语言](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
| [293](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [位置API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)为Java ME 2.0 |
| [294](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |                在Java编程语言的改进模块化支持                |
| [296](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) | [Swing应用程序框架](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（Java SE 7中） |
| [299](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |                Java的上下文和依赖注入（焊接）                |
|      [301](http://www.360doc.com/content/18/0916/08/01)      |                      JSF Portlet的桥梁                       |
|      [303](http://www.360doc.com/content/18/0916/08/03)      | [Bean验证](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [307](http://www.360doc.com/content/18/0916/08/07)      | [移动网络和移动数据API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（截至7月正式计划，20日，2007年，但官方发布2。问：2008 |
|      [308](http://www.360doc.com/content/18/0916/08/08)      | [注解](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)的Java类型（Java SE的8） |
|      [311](http://www.360doc.com/content/18/0916/08/11)      | [RESTful Web服务的Java API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JAX-RS的）1.0和1.1 |
|      [314](http://www.360doc.com/content/18/0916/08/14)      | [的JavaServer Faces](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JSF）的2.0 |
|      [316](http://www.360doc.com/content/18/0916/08/16)      | [的Java平台企业版](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（Java EE）的6 |
|      [317](http://www.360doc.com/content/18/0916/08/17)      | [Java持久性API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JPA）的2.0 |
|      [322](http://www.360doc.com/content/18/0916/08/22)      | [Java EE连接器架构](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JCA）的1.6 |
|      [325](http://www.360doc.com/content/18/0916/08/25)      | [IMS通信促成](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（ICE）的（见[的IMS](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)） |
|      [330](http://www.360doc.com/content/18/0916/08/30)      | [对Java的依赖注入](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [343](http://www.360doc.com/content/18/0916/08/43)      | [Java消息服务](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) 2.0（JMS） |
|      [354](http://www.360doc.com/content/18/0916/08/54)      | [Java的货币及货币的API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|                                                              |                                                              |
|      [901](http://www.360doc.com/content/18/0916/08/01)      | [Java语言](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)规范，第三版（JLS的）（J2SE 5.0的集成的JSR 14，41，133，175，201和204） |
|      [907](http://www.360doc.com/content/18/0916/08/07)      | [Java事务API](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JTA），1.0和1.1 |
|      [912](http://www.360doc.com/content/18/0916/08/12)      | [Java 3D的](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API 1.3 |
|      [913](http://www.360doc.com/content/18/0916/08/13)      | Java社区进程（JCP）的2.0，2.1和2.5。[[ 8 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [914](http://www.360doc.com/content/18/0916/08/14)      | [Java消息服务](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)（JMS）API的1.0和1.1 |
|      [924](http://www.360doc.com/content/18/0916/08/24)      | 第二版（JVM）[Java虚拟机](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml)规范（J2SE 5.0的）。[[ 9 \]](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) |
|      [926](http://www.360doc.com/content/18/0916/08/26)      | [的Java 3D](http://www.360doc.com/content/18/0916/08/9200790_787041951.shtml) API 1.5 |



**JSR269 可插拔的注解处理API**，其原理如下：


<div align='center'><img src=./img/手把手实现一个lombok/手把手实现一个lombok_2023-04-10-21-03-37.png width='100%'/></div><br/>

<div align='center'><img src=./img/手把手实现一个lombok/手把手实现一个lombok_2023-04-10-21-04-14.png width='100%'/></div><br/>

## 二、实现步骤

### 1.工程与环境依赖 

1. 配置maven 插件，pom.xml  编译参数

``` xml
<build>

        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.8.0</version>
                <configuration>
                    <source>8</source>
                    <target>8</target>
                    <encoding>UTF-8</encoding>
                    <compilerArgs>
                        <arg>-parameters</arg>
                        <arg>-proc:none</arg>
                        <arg>-XDignore.symbol.file</arg>
                    </compilerArgs>
                    <compilerArguments>
                        <bootclasspath>
                            ${java.home}/lib/rt.jar${path.separator}${java.home}/lib/jce.jar${path.separator}${java.home}/../lib/tools.jar
                        </bootclasspath>
                    </compilerArguments>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>
```

#### 注意细节

1. Lombok项目本身要加 编译 参数 ，防止编译处理器无法实例化：-proc:none
2. 要添加编译 类路径 bootclasspath： 指定tool.jar
3. 在测试的时候 要构建一个新的工程，用一个新的IDEA窗口打开

### 2.注解处理器

- [ ] 打印编译信息 

1. 编写注解处理器,实现`AbstractProcessor`
2. 基于SPI指定处理器的路径 ：工程/resources/META-INF/services/javax.annotation.processing.Processor
3. 打印消息的时候，maven 用System.out,  idea用`Messager` 类

``` java
@SupportedSourceVersion(SourceVersion.RELEASE_8)
@SupportedAnnotationTypes("org.myLombok.Hello")
public class HelloProcessor  extends AbstractProcessor {
    @Override
    public synchronized void init(ProcessingEnvironment processingEnv) {
        System.out.println("这是我的第一人编译注释处理器");
        processingEnv.getMessager().printMessage(Diagnostic.Kind.NOTE,"这是我的处理器");
    }

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        return false;
    }
}
```



### 3.jcTree 修改语法

- [ ] 构建一个hello world 语句

``` java
package org.myLombok;
/**
 * @Copyright 源码阅读网 http://coderead.cn
 */

import com.sun.tools.javac.api.JavacTrees;
import com.sun.tools.javac.processing.JavacProcessingEnvironment;
import com.sun.tools.javac.tree.JCTree;
import com.sun.tools.javac.tree.TreeMaker;
import com.sun.tools.javac.tree.TreeTranslator;
import com.sun.tools.javac.util.Context;
import com.sun.tools.javac.util.List;
import com.sun.tools.javac.util.Names;

import javax.annotation.processing.*;
import javax.lang.model.SourceVersion;
import javax.lang.model.element.TypeElement;
import javax.tools.Diagnostic;
import java.util.Set;

/**
 * @author 鲁班大叔
 * @date 2023
 */
@SupportedSourceVersion(SourceVersion.RELEASE_8)
@SupportedAnnotationTypes("org.myLombok.Hello")
public class HelloProcessor  extends AbstractProcessor {
    private JavacTrees javacTrees; // 获取 JcTree
    private TreeMaker treeMaker; // 构建生成 jcTree
    private Names names;

    @Override
    public synchronized void init(ProcessingEnvironment processingEnv) {
        System.out.println("这是我的第一人编译注释处理器");
        processingEnv.getMessager().printMessage(Diagnostic.Kind.NOTE,"这是我的处理器");
        javacTrees = JavacTrees.instance(processingEnv);// 语法树
        Context context = ((JavacProcessingEnvironment) processingEnv).getContext();
        this.treeMaker = TreeMaker.instance(context);
        super.init(processingEnv);
        this.names = Names.instance(context);

    }

    @Override
    public boolean process(Set<? extends TypeElement> annotations, RoundEnvironment roundEnv) {
        annotations.stream()
                .flatMap(t->roundEnv.getElementsAnnotatedWith(t).stream())
                .forEach(t->{
                    JCTree tree = javacTrees.getTree(t);
                 // 基于访问者设计模式 去修改方法
                    tree.accept(new TreeTranslator(){
                        @Override
                        public void visitMethodDef(JCTree.JCMethodDecl tree) {
//                            System.out.println("hello world");
                            JCTree.JCStatement sysout = treeMaker.Exec(
                                    treeMaker.Apply(
                                            List.nil(),
                                            select("System.out.println"),
                                            List.of(treeMaker.Literal("hello world!")) // 方法中的内容
                                    )
                            );
                            // 覆盖原有的语句块
                            tree.body.stats=tree.body.stats.append(sysout);
                            super.visitMethodDef(tree);
                        }
                    });
                });

        return true;
    }


    private JCTree.JCFieldAccess select(JCTree.JCExpression selected, String expressive) {
        return treeMaker.Select(selected, names.fromString(expressive));
    }

    private JCTree.JCFieldAccess select(String expressive) {
        String[] exps = expressive.split("\\.");
        JCTree.JCFieldAccess access = treeMaker.Select(ident(exps[0]), names.fromString(exps[1]));
        int index = 2;
        while (index < exps.length) {
            access = treeMaker.Select(access, names.fromString(exps[index++]));
        }
        return access;
    }

    private JCTree.JCIdent ident(String name) {
        return treeMaker.Ident(names.fromString(name));
    }
}

```

