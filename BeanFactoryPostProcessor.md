- [BeanFactoryPostProcessor](#BeanFactoryPostProcessor)
  - [自定义BeanFactoryPostProcessor](#自定义BeanFactoryPostProcessor)
## BeanFactoryPostProcessor

> * BeanFactory后置处理器
> * BeanFactory标准初始之后调用，bean定义加载到BeanFactory之后，bean创建对象之前
> * 实现BeanFactoryPostProcessor接口，重写postProcessBeanFactory方法

### 自定义BeanFactoryPostProcessor

myBeanFactoryPostProcessor.java
```java
@Component
public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
        System.out.println("postProcessBeanFactory...");
    }
}
```

