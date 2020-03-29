# Spring 基于注解的AOP配置；



## 常见4种 基于注解的AOP配置中出现的顺序调用问题（可能没有修复）：

### 基于xml的配置 bean.xml

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd">
    <!-- 配置spring创建容器时要扫描的包-->
    <context:component-scan base-package="com.itheima"></context:component-scan>
    <!-- 配置Spring 开启注解AOP的支持-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

### 账户业务实现类

```java
/**
 * 账户业务层 实现类
 */
@Service("accountService")
public class AccountServiceImp implements IAccountService {

    @Override
    public void saveAccount() {

        System.out.println("执行了保存");
        int i=1/0;
    }

    @Override
    public void updateAccount(int i) {
        System.out.println("执行了更新");
    }

    @Override
    public void deleteAccount() {
        System.out.println("执行了删除");
    }
}
```

### 通知类

```java
/**
 * 用于记录日志的工具类，它里面提供了公共的代码。
 */
@Component("logger")
@Aspect  //表示当前类是一个切面类
public class Logger {

    @Pointcut("execution(* com.itheima.service.impl.*.*(..))")
    private void pt1(){

    }
    /**
     * 前置通知
     */
   // @Before("pt1()")
    public void beforePrintLog(){
        System.out.println("前置通知Logger类中的beforePrintLog方法开始记录日志了......");
    }

    /**
     * 后置通知
     */

    //@AfterReturning("pt1()")
    public void afterReturnPrintLog(){
        System.out.println("后置通知Logger类中的afterReturnPrintLog方法开始记录日志了......");
    }

    /**
     * 异常通知
     */
    //@AfterThrowing("pt1()")
    public void afterThrowingPrintLog(){
        System.out.println("异常通知Logger类中的afterThrowingPrintLog方法开始记录日志了......");
    }
    /**
     * 前置通知
     */
    //@After("pt1()")
    public void afterPrintLog(){
        System.out.println("最终通知Logger类中的afterPrintLog方法开始记录日志了......");
    }

    /**
     * 环绕通知
     * 问题：
     *      当我们配置环绕通知之后，切入点方法没有执行，而通知方法执行了。
     * 分析：通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，而我们代码中没有。
     * 解决：
     *     Spring框架为我们提供了一个接口:ProceedingJoinPoint。该接口有一个方法proceed(),此方法就相当于明确调用切入点方法。
     *     该接口可以作为环绕通知的方法参数,在程序执行时，spring框架会为我们提供该接口的实现类提供我们使用。
     *
     * Spring环绕通知：
     *  它是spring框架为我们提供的一种可以在代码中手动控制增强方法 何时增强的方式。
     *  除了通过配置实现，也可以通过
     */
    @Around("pt1()")
    public Object aroundPrintLog(ProceedingJoinPoint pjp){
        Object rtValue= null;
        try {
            System.out.println("Logger 类中的aroundPrintLog方法开始记录日志.....前置.");
            Object[] args=pjp.getArgs();
            rtValue = pjp.proceed(); //明确调用业务层方法（切入点方法）
            System.out.println("Logger 类中的aroundPrintLog方法开始记录日志.....后置.");
            return rtValue;
        } catch (Throwable throwable) {
            System.out.println("Logger 类中的aroundPrintLog方法开始记录日志.....异常.");
            throw new RuntimeException(throwable);
        } finally {
            System.out.println("Logger 类中的aroundPrintLog方法开始记录日志.....final.");
        }
    }
}
```



## 使用环绕通知的方法，
环绕、通知的代码是自己编写的，不太容易出现问题。




###  不使用XML的配置方式
```java
@Configuration 
@ComponentScan(basePackages="com.itheima")
@EnableAspectJAutoProxy 
public class SpringConfiguration {
}
```
