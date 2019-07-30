#日志

##日志框架
- 日志门面（抽象层呢个） SLF4j
- 日志实现 Logback
- SpringBoot底层是Spring框架 默认用JCL
    SpringBoot选择SLF4j和logback
    
##SLF4j使用 
- 调用
    官方文档：https://www.slf4j.org  
    日志记录方法的调用 不应该直接调用日志的实现类 而是调用日志抽象层的方法  
    给系统里导入slf4j的jar和logback的实现jar
    ```java
    import org.slf4j.logger;
    import org.slf4j.LoggerFactory;
    
    public class HelloWorld{
      public static void main(String[] args){
        Logger logger=LoggerFactory.getLogger(HelloWorld.class);
        logger.info("Hello World");
    }
  }

- 每一个日志的实现框架都有自己的配置文件 使用slf4j以后 配置文件还是做成日志实现框架的配置文件

## 遗留问题
- a项目（slf4j+logback）：使用别的框架 用到别的日志框架
- 统一日志记录
    - 将系统中其他日志框架先排除出去
    - 用中间包替换原有的日志框架
    - 我们导入slf4j其他的实现
    
## SpringBoot日志关系
```xml
    <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter</artifactId>
          <version>2.1.6.RELEASE</version>
          <scope>compile</scope>
    </dependency>
```
- 日志功能
```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-logging</artifactId>
      <version>2.1.6.RELEASE</version>
      <scope>compile</scope>
    </dependency>
```
- 底层依赖关系
    ![avatar](/Users/lyj1996/Documents/markdown/pictures/logging1.png)
    把其他日志替换成了slf4j
- SpringBoot能自动适配所有的日志 引入其他框架时候只需要把这个框架依赖的日志框架排除掉

##SpringBoot日志使用
- 默认配置
    默认配置了日志
    ```java
          //记录器
    	Logger logger= LoggerFactory.getLogger(getClass());
    	@Test
    	public void contextLoads() {
    		//日志级别 由低到高
    		//可以调整输出的日志级别 日志就会在这之后生效
    
    		logger.trace("这是trace日志。。。");
    		logger.debug("这是debug日志。。。");
    		//默认info级别 root级别
    		logger.info("这是info日志。。。");
    		logger.warn("这是warn日志。。。");
    		logger.error("这是error日志。。。");
    	}

- 日志级别修改
    logging.level.com.example=trace

- 文件位置
    logging.file  none  指定文件名  none
    logging.path  none  none      指定目录
    日志输出       控制台  输出到文件  输出到目录下的spring.log文件
  
    当前项目下生成springboot.log 也可以指定路径
    logging.file=springboot.log 
    
    logging.path=
    
    在控制台输出的日志的格式
    logging.pattern.console=
    在文件中日志输出的格式
    logging.pattern.file=
- 指定配置
    在类路径下放上每个日志框架自己的配置文件 此时SpringBoot不使用默认配置
    logback.xml 直接被日志框架识别 
    logback-spring.xml 日志框架不直接加载配置项 由SpringBoot加载 可以使用springProfile
    ```xml
    <springProfile name="staging">
    可以指定某段配置只在每个环境下生效
    </springProfile>

##切换日志框架 
- 按slf4j日志适配图进行相关的切换