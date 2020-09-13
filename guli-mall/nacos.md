### `Nacos`服务注册和发现

------

- **`Nacos`文档地址**

  文档地址：`https://nacos.io/zh-cn/docs/quick-start-spring-cloud.html`

  `github`地址：`https://github.com/alibaba/nacos`

  spring boot与spring cloud版本对应：`https://start.spring.io/actuator/info`

- **引入依赖**

  ```xml
  <!--服务注册和发现-->
  <dependency>
      <groupId>com.alibaba.cloud</groupId>
      <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
  </dependency>
  ```

- **安装`Nacos`**

  参照文档。

- **`yaml`配置**

  ```yaml
  spring:
      cloud:
          nacos:
            discovery:
              server-addr: 127.0.0.1:8848
        application:
          name: gulimall-coupon
  ```

### Feign远程调用

------

- **引入依赖**

  服务调用方和提供方都引入依赖。

  ```xml
  <dependency>
  	<groupId>org.springframework.cloud</groupId>
  	<artifactId>spring-cloud-starter-openfeign</artifactId>
  </dependency>
  ```

- **服务提供方提供接口**

  和之前的HTTP接口方式一样，没有什么特别的。

-   **服务消费方**

    创建与提供方相同签名的接口。其他地方注入直接使用即可。

    ```java
    // 对应服务提供方的application.name的名字
    @FeignClient("gulimall-coupon")
    public interface CouponFeignService {
        // 方法的签名与提供方保持一致
        @RequestMapping("coupon/coupon/list")
        R list(@RequestParam Map<String, Object> params);
    }
    ```

### `Nacos`配置中心

------

- **引入依赖**

  服务调用方和提供方都引入依赖。

  ```xml
  <dependency>
      <groupId>com.alibaba.cloud</groupId>
      <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
  </dependency>
  ```

- **`bootstrap.properties`中配置 `Nacos Server` 的地址和应用名**

  在resource中创建`bootstrap.properties`并配置地址和应用名。注意在`nacos`配置中心的`dataId`与应用名一致。

  ```properties
  spring.cloud.nacos.config.server-addr=127.0.0.1:8848
  spring.application.name=gulimall-coupon
  ```

- **配置自动更新**

  通过 Spring Cloud 原生注解 `@RefreshScope` 实现配置自动更新：

  ```java
  @RestController
  @RequestMapping("/config")
  @RefreshScope
  public class ConfigController {
  
      @Value("${useLocalCache:false}")
      private boolean useLocalCache;
  
      @RequestMapping("/get")
      public boolean get() {
          return useLocalCache;
      }
  }
  ```
  
- **命名空间**

  配置环境隔离。

  ```properties
  spring.cloud.nacos.config.namespace=b3404bc0-d7dc-4855-b519-570ed34b62d7
  ```

- **配置分组**

  在创建配置时手动输入配置分组。

  ```properties
  spring.cloud.nacos.config.group=DEVELOP_GROUP
  ```

- **总结**

  1.所有配置都可以放入配置中心中，只需要在bootstrap.properties中指明从哪获取配置即可

  2.配置优先从配置中心中获取，如果配置中心没有则从本地获取。

  

