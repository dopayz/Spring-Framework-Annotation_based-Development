- [BeanDefinitionRegistryPostProcessor](#BeanDefinitionRegistryPostProcessor)
  - [自定义BeanDefinitionRegistryPostProcessor](#自定义BeanDefinitionRegistryPostProcessor)

## BeanDefinitionRegistryPostProcessor

> * bean定义注册后置处理器
> * bean尚未加载到BeanFactory中，当然bean也未初始化，早于BeanFactoryPostProcessor执行
> * 实现BeanDefinitionRegistryPostProcessor接口，重写postProcessBeanDefinitionRegistry、postProcessBeanFactory方法

### 自定义BeanDefinitionRegistryPostProcessor

MyBeanDefinitionRegistryPostProcessor.java

```java
@Component
public class MyBeanDefinitionRegistryPostProcessor implements BeanDefinitionRegistryPostProcessor {
    //先于postProcessBeanFactory执行
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry beanDefinitionRegistry) throws BeansException {
        System.out.println("postProcessBeanDefinitionRegistry...bean数量："+beanDefinitionRegistry.getBeanDefinitionCount()+"");
        RootBeanDefinition rootBeanDefinition = new RootBeanDefinition(ExtTestObj.class);
        //手工注册bean，自定义bean的id
        beanDefinitionRegistry.registerBeanDefinition("myExt",rootBeanDefinition);
    }

    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
        System.out.println("postProcessBeanFactory...bean数量："+configurableListableBeanFactory.getBeanDefinitionCount()+"");
    }
}
```



