- [AOP](#AOP)
  - [实现三步骤](#实现三步骤)
  - [业务类](#业务类)
  - [切面类](#切面类)
    - [环绕通知](#环绕通知)
  - [配置类](#配置类)
## AOP
> * 指在程序运行期动态的将某段代码切入到指定方法指定位置运行的编程方式
> * 依赖于Spring的aop组件：spring-aspects

### 实现三步骤
> * 将业务逻辑组件和切面类组件都注册进容器中，并用@Aspect标注哪个是切面类
> * 在每个通知方法上标注通知注解，告诉spring何时何地运行(切入点表达式)
> * 启动基于注解方式的aop:@EnableAspectJAutoProxy

### 业务类

编写一个数学计算器业务类
```java
public class MathCalculator {
    //除法
    public int div(int i,int j){
        return i/j;
    }
}
```

### 切面类

> * 前置通知(@Before)：方法之前执行，例如创建连接对象
> * 后置通知(@After)：方法返回结果之前执行，例如关闭连接对象
> * 返回通知(@AfterReturning)：方法执行完之后，例如提交事务
> * 异常通知(@AfterThrowing)：方法抛出异常之后，例如回滚事务
> * 环绕通知(@Around):最强大的通知，因为他可以将上面的几个通知融合到一起
> * 通知也可以说增强

```java
@Aspect   //切面类
public class LogAspects {
    //@Pointcut切入点，以及表达式
    @Pointcut("execution(public int com.zy.aop.MathCalculator.*(..))")
    public void pointCut(){};

    //@Before:前置通知，方法之前执行
    @Before("pointCut()")
    public void logStart(JoinPoint joinPoint){
        Object[] args = joinPoint.getArgs();
        System.out.println(joinPoint.getSignature().getName()+"开始....{"+ Arrays.asList(args)+"}");
    }
    //@After:后置通知，方法返回结果之前执行
    @After("pointCut()")
    public void logEnd(JoinPoint joinPoint){
        System.out.println(joinPoint.getSignature().getName()+"结束...");
    }
    //@AfterReturning:返回通知,方法执行完之后
    @AfterReturning(value = "pointCut()",returning = "result")
    public void logReturn(JoinPoint joinPoint,Object result){
        System.out.println(joinPoint.getSignature().getName()+"结果....{"+result+"}...");
    }
    //@AfterThrowing：异常通知,方法抛出异常之后
    @AfterThrowing(value = "pointCut()",throwing = "exception")
    public void logException(JoinPoint joinPoint,Exception exception){
        System.out.println(joinPoint.getSignature().getName()+"异常....{"+exception+"}...");
    }
}
```
#### 环绕通知
  ```java
@Aspect   //切面类
public class LogAspects {
    //@Pointcut切入点，以及表达式
    @Pointcut("execution(public * com.zy.aop.M*.*(..))")
    public void pointCut(){};

   @Around(value = "pointCut()")
    public Object around(ProceedingJoinPoint pjp){
        Object[] args = pjp.getArgs();
        String methodName = pjp.getSignature().getName();

        System.out.println(methodName+"开始执行...参数：{"+Arrays.asList(args)+"}");
        Object obj = null;
        try {
            obj = pjp.proceed(args);//执行目标方法
            System.out.println(methodName+"返回值：{"+obj+"}");
        } catch (Throwable throwable) {
            System.out.println(methodName+"异常：{"+throwable+"}");
            throwable.printStackTrace();
        }
        System.out.println(methodName+"执行结束...");
        return obj;
    }
}
```

### 配置类

```java
@EnableAspectJAutoProxy //开启Spring的aop注解模式
@Configuration
public class MainConfig {
    @Bean
    public MathCalculator mathCalculator(){
        return new MathCalculator();
    }

    @Bean
    public LogAspects logAspects(){
        return new LogAspects();
    }
}
```

单元测试
```java
public class IOC_AOPTest {
    @Test
    public void test1(){
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig.class);
        MathCalculator mathCalculator = (MathCalculator)context.getBean("mathCalculator");
        mathCalculator.div(5,2);
    }
}
```
