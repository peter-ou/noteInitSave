
### 1,aop的本质：

我的总结：本质是利用jdk的反射机制，获取拦截方法/构造方法/属性的基类AccessibleObject, 然后在这个AccessibleObject类可以破坏修饰符private的权限，从而获取上面的所有信息。从而可以实现对目标方法/构造方法/属性的调用；而在调用这些之前或者之后，我们还可以增强一些我们的其他方法和操作等。

关键的类有：Joinpoint , AccessibleObject是解决获取目标对象问题；例如MethodInterceptor拦截器他拦截的就是MethodInvocation类，而这个MethodInvocation类的本质就是通过其父类接口Joinpoint的AccessibleObject getStaticPart();方法获取目标方法对象Method；

而继承Advice接口的拦截器Interceptor拦截的实现底层原理待解决？

资料参考：
spring官网介绍：
https://docs.spring.io/spring-framework/docs/6.0.8-SNAPSHOT/reference/html/core.html#aop-api
Spring Aop介绍
https://www.baeldung.com/spring-aop
实现一个自定义的Spring Aop注解
https://www.baeldung.com/spring-aop-annotation
切点表达式详解
https://www.baeldung.com/spring-aop-pointcut-tutorial
Spring的Advice类型介绍
https://www.baeldung.com/spring-aop-advice-tutorial
AspectJ介绍
https://www.baeldung.com/aspectj
使用AspectJ为带注释的类提供增强方法
https://www.baeldung.com/aspectj-advise-methods
Joinpoint vs ProceedingJoinPoint
https://www.baeldung.com/aspectj-joinpoint-proceedingjoinpoint
Spring Performance Logging
https://www.baeldung.com/spring-performance-logging
When Does Java Throw UndeclaredThrowableException
https://www.baeldung.com/java-undeclaredthrowableexception
Get Advised Method Info in Spring AOP
https://www.baeldung.com/spring-aop-get-advised-method-info

