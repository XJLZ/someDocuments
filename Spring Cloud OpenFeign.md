---

---

#### OpenFeign简介

[官网]: https://cloud.spring.io/spring-cloud-static/Hoxton.SR1/reference/htmlsingle/#spring-cloud-openfeign

​		OpenFeign是springcloud在Feign的基础上支持了SpringMVC的注解，如@RequestMapping等等。OpenFeign的@FeignClient可以解析SpringMVC的@RequestMapping注解下的接口，并通过动态代理的方式产生实现类，实现类中做负载均衡并调用其他服务。

##### 使用方式

​		在注册中心这个微服务中引入依赖

```xml
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>
```

​		在application.yml中添加配置

```yml
#设置feign 客户端超时时间（openFeign默认支持ribbon）
ribbon:
  #指的是建立连接所用的时间，适用于网络状况正常的情况下，两端连接所用的时间 单位：毫秒
  ReadTimeout: 5000
  #指的是建立连接后从服务器读取到可用资源所用的时间,单位：毫秒
  ConnectTimeout: 5000
  
 # 设置回退生效
feign:
  hystrix:
    enabled: true
    
hystrix:
  command:
    default: #也可以针对多个服务
      execution:
        isolation:
          thread:
            timeoutInMilliseconds: 5000 # 设置hystrix的超时时间为5s
```

新建一个接口

```java
@FeignClient(value = "consul-provider-payment", contextId = "helloServiceClient", fallbackFactory = HelloServiceFallback.class)
public interface IHelloService {

    @GetMapping("hello")
    String hello(@RequestParam("name") String name);

    @GetMapping("user")
    String info();
}
```



1.`value`指定资源服务的名称，通常为资源服务配置里 `spring.application.name`的值;

2.`contextId`指定这个Feign Client的别名，当我们定义了多个Feign Client并且`value`值相同（即调用同一个服务）的时候，需要手动通过`contextId`设置别名，否则程序将抛出异常；`

3.`fallbackFactory`指定了回退方法，当我们调用远程服务出现异常时，就会调用这个回退方法。`fallback`也可以指定回退方法，但`fallbackFactory`指定的回退方法里可以通过`Throwable`对象打印出异常日志，方便分析问题

在接口下新建一个fallback包，进入，然后在该包下新建一个HelloServiceFallback

```java
@Slf4j
@Component
public class HelloServiceFallback implements FallbackFactory<IHelloService> {
    @Override
    public IHelloService create(Throwable throwable) {
        return new IHelloService() {
            @Override
            public String hello(String name) {
                log.error("调用server-system-hello服务出错", throwable);
                return "调用出错";
            }

            @Override
            public String info() {
                log.error("调用server-system-info服务出错", throwable);
                return null;
            }
        };
    }
}
```

最后在启动类上添加注解：**@EnableFeignClients**