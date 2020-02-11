**SpringBoot在启动时会自动扫描该启动类所在包及其子包的所有子类**


### SpringBoot默认配置文件
1. application.yml或者是application.properties,SpringBoot启动时会自动扫描这些配置文件

### JavaBean中属性注入的内容:使用默认配置文件的内容
2. @ConfigurationProperties(prefix = "person")为javabean批量注入属性
3. @Value属性能够注入单个属性
4. @Validated:检验插入数据的类型

###为JavaBean属性注入声明配置文件的来源
1. @PropertySource(value = {"classpath:person.properties"})声明配置文件的来源

### 给SpringBoot添加额外的配置文件,可以使用注解
@ImportResource导入其他的配置文件

可以创建一个配置类,在这个类上面加上@Configuration注解,在配置类中加上@Bean注解添加组件


在配置文件中可以使用之前已经配置好的属性

### SpringBoot加载的配置文件的默认位置及其优先级,默认都是application.properties/yml


### 可以在配置文件中编写的内容

可以查看SpringBoot自动配置的原理,在配置文件声明的内容,能够作用到要注入到容器中对象的属性

SpringBoot会存在许多的自动注入的组件,会在容器中注入bean对象

这里以HttpEncodingAutoConfiguration为例,这个类上面有一个注解@EnableConfigurationProperties({HttpProperties.class}),让里面参数表明的类进行属性自动注入,也就是HttpProperties进行属性自动注入,然后进入其中可以看出配置属性是直接从主配置文件中读取对应的属性进行注入的

自动配置的组件会被加入到容器中去,然后自动配置组件中的@EnableConfigurationProperties({HttpProperties.class})注解会将属性注入到HttpProperties的bean对象中,并且HttpProperties对象会被注入到容器中去

并且类中如果只有一个有参构造器,那么这个类中所需要的参数将会到容器中去寻找

在返回过滤器时,属性值会从上面配置类中的属性获取到,而且已经与配置文件的内容相匹配,所以这个返回的过滤器的字符集与我们在配置文件中配置的内容的值保持一致
