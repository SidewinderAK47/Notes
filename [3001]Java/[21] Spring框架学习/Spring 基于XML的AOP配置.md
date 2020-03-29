# Spring 基于XML的AOP配置
## 账户业务接口类
```java
/**
 * 账户业务接口层
 */
public interface IAccountService {
    /**
     * 模拟保存账户
     */
    void saveAccount();

    /**
     * 模拟更新账户
     * @param i
     */
    void updateAccount(int i);

    /**
     * 模拟删除账户
     * @param i
     */
    void deleteAccount();
}
```


## 模拟账户业务实现类
```java
public class AccountServiceImp implements IAccountService {
    @Override
    public void saveAccount() {
        System.out.println("执行了保存");
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
## 日志类（通知类）
```java
/**
 * 用于记录日志的工具类，它里面提供了公共的代码。
 */
public class Logger {
    /**
     * 用于打印日志，计划其在切入点方法执行之前（切入点方法就是业务层方法）
     */
    public void printLog(){
        System.out.println("Logger类中的printLog方法开始记录日志了......");
    }
}
```
### 基于xml的AOP配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--配置Spring的IOC,把service对象配置进来-->
   <bean id="accountService" class="com.itheima.service.impl.AccountServiceImp"></bean>

    <!--Spring的 基于xml的AOP配置步骤
        1.把通知bean也要交给Spring来管理
        2.使用aop：config标签表明开始AOP的配置。
        3.使用aop标签表明配置切面。
            id属性：给切面提供为题标志；
            ref属性：是指指定通知类bean的id；
        4.在aop:aspect 标签内部使用对应的标签来配置通知类型。
        5.我们现在示例是让printLog方法在切入点之前执行，所以是前置通知
        aop:before:表示前置通知
            method 属性:用于指定Logger类中的哪个方法是前置通知。
            pointercut属性: 用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法进行增强。

           切入点表达式写法：
                关键字：execution(表达式)
                表达式：
                    访问修饰符 返回值 包名.包名.包名...类名.方法名(参数列表)
                标准的表达地写法：
                    public void com.itheima.service.impl.AccountServiceImpl.saveAccount()
               首先：访问修饰符可以省略
                    void com.itheima.service.impl.AccountServiceImpl.saveAccount()
               返回值可以使用通配符，表示任何返回值
               * com.itheima.service.impl.AccountServiceImpl.saveAccount();
               包名可以使用通配符，表示任意包，有几集包，就要写几个星
               * *.*.*.*.AccountServiceImpl.saveAccount();
               包名.. 可以表示当前包及其子包
               * *..AccountServiceImpl.saveAccount() : 任意包下，只有一个这个类与方法都会
                类名和方法都是通过*号，实现通配：
                * *..*.*()
                参数列表：
                    可以直接写数据类型
                        基本类型直接写名称 int
                        引用类型写包名，类名的方式 java.lang.String
                    类型可以使用通配符，表示任意类型，但是必须有参数。
                    可以使用..表示有无参数均可。

               全通配写法：
                * *..*.*(..)

               实际开发过程中，切入点表达式的通常写法：
                    切到业务实现类下的所有方法
                    * com.itheima.service.impl.*.*(..)
    -->

    <!--配置Logger类-->
    <bean id="logger" class="com.itheima.utils.Logger"></bean>

    <!--配置AOP-->
    <aop:config>
        <!--配置了一个切面-->
        <aop:aspect id="logAdvice" ref="logger">
            <!--配置通知的类型 ，并且建立通知方法与切入方法的关联-->
            <!--com.itheima.service.impl-->
            <!--<aop:before method="printLog" pointcut="execution(public void com.itheima.service.impl.AccountServiceImp.saveAccount())"></aop:before>-->
            <aop:before method="printLog" pointcut="execution(* com.itheima.service.impl.*.*(..))"></aop:before>
        </aop:aspect>
    </aop:config>

    <!--小知识：aspectjweaver 是一个软件联盟，负责解析 切入点表达式写法 -->
</beans>
```
### 测试代码
```java
public class AOPTest {
    public static void main(String[] args) {
        //1.获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.获取对象
        IAccountService accountService = ac.getBean("accountService",IAccountService.class);
        //3.执行方法
        accountService.saveAccount();
        accountService.updateAccount(1);
        accountService.deleteAccount();
    }
}
```
### pom.xml
```xml
	<dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.3.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.9.5</version>
        </dependency>
```



## 四种常见的通知类型

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--配置Spring的IOC,把service对象配置进来-->
   <bean id="accountService" class="com.itheima.service.impl.AccountServiceImp"></bean>

    <!--配置Logger类-->
    <bean id="logger" class="com.itheima.utils.Logger"></bean>

    <!--配置AOP-->
    <aop:config>
        <!--这句话如果放置在切面外面，需要放在切面之前，因为xml配置文件要有配置的要求 -->
        <aop:pointcut id="pt1" expression="execution(* com.itheima.service.impl.*.*(..))"></aop:pointcut>
        <!--配置了一个切面-->
        <aop:aspect id="logAdvice" ref="logger">
            <!--配置 前置通知   在切入点方法执行 之前-->
            <aop:before method="beforePrintLog" pointcut-ref="pt1"></aop:before>

            <!--后置 前置通知  在切入点方法正常执行之后 它和异常通知永远只能执行一个 -->
            <aop:after-returning method="afterReturnPrintLog" pointcut-ref="pt1"></aop:after-returning>

            <!--配置 前置通知  在切入点方法执行产生异常之后执行  它和后置通知永远只能执行一个-->
            <aop:after-throwing method="afterThrowingPrintLog" pointcut-ref="pt1"></aop:after-throwing>

            <!--配置 前置通知  无论切入点方法是否正确执行，都会在其后面执行-->
            <aop:after method="afterPrintLog" pointcut-ref="pt1"></aop:after>


            <!--配置切入点表达式标签
                id属性用于指定表示的唯一标志。 expression属性用于指定表达式的内容
                此标签写在aop：aspect标签内部只能当前切面是用
                它还可以写在aop:aspect外面，此时就变成了所有切面可以用
            -->
            <!--<aop:pointcut id="pt1" expression="execution(* com.itheima.service.impl.*.*(..))"></aop:pointcut>-->
        </aop:aspect>
    </aop:config>

    <!--小知识：aspectjweaver 是一个软件联盟，负责解析 切入点表达式写法 -->
</beans>
```

