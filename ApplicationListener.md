- [ApplicationListener](#ApplicationListener)
  - [自定义ApplicationListener](#自定义ApplicationListener)

## ApplicationListener

> * 自定义监听器，需实现ApplicationListener接口，重写onApplicationEvent
> * 在容器启动时，发布自定义事件
> * 监听器则会监听到发布的自定义事件

### 自定义ApplicationListener

myApplicationListener
```java
@Component
public class MyApplicationListener implements ApplicationListener {
    public void onApplicationEvent(ApplicationEvent event) {
        System.out.println("订阅："+event);
    }
}
```

单元测试
```java
public class IOC_ExtTest {
    @Test
    public void test(){
        AnnotationConfigApplicationContext context =
                new AnnotationConfigApplicationContext(MainConfig.class);
        context.publishEvent(new ApplicationEvent(new String("我发布了事件")) {});
        context.close();
    }
}
```
