
- [SSM](#ssm)
  - [1.Spring](#1spring)
    - [1.1 IOC:(Inversion(反转) Of Control)：控制反转；](#11-iocinversion反转-of-control控制反转)
    - [1.2 DI：（Dependency Injection）依赖注入;](#12-didependency-injection依赖注入)
    - [1.3 bean](#13-bean)
    - [1.4 AOP](#14-aop)
----
# SSM
## 1.Spring
- IOC（容器）  
　Struts2
　Hibernate
　Mybatis  
- AOP（面向切面编程） 声明式事务  
　Spring-JdbcTemplate  

### 1.1 IOC:(Inversion(反转) Of Control)：控制反转；  
- 控制：资源的获取方式
    - 主动式：（需要什么资源自己创建）  
        ```
        BookServlet{BookService bs = new BookService();}
        ```
    - 被动式：（）
        ```
        BookServlet{
            BookService bs;
            public void test01(){
                bs.checkout();
            }
        }
        ```  
- 容器：管理所有的组件（有功能的类）；假设，BookServlet受容器管理，BookService也受容器管理；容器可以自动的探查出那些组件（类），需要用到另一些组件（类）；容器帮我们创建BookService对象，并把BookService对象赋值过去；  

容器：主动的new资源变为被动接受资源；  
（容器）婚介所；  
- 主动获取变为被动接受；  

### 1.2 DI：（Dependency Injection）依赖注入;  
- 容器能知道哪个组件（类）运行的时候，需要另一个类（组件）;容器通过反射的形式，将容器中准备好的BookService对象注入（利用反射给属性赋值）到BookServlet中；
- 只要容器管理的组件，都能使用容器提供强大的功能；
- 框架编写流程：
  ```
  1. 导包
    核心容器：
    spring-bean-....jar
    ...context-....jar
    ...core....jar
    ...expression....jar
  2. 写配置
    Spring的配置文件中，集合了spring的ioc容器管理的所有组件（会员清单）;
    创建一个Spring Bean Configuration File(Spring的bean配置文件)
    ioc.xml
    <!-- 使用property标签为Person对象的属性赋值
        name="lastName"：指定属性名
        value="张三"：为这个属性赋值
     -->
    <beans ...>
        <bean id="person01 " class="com.*.*.Person">
            <property name="lastname" value="张三"></property>
            <property name="age" value="12"></property>
        </bean>
    </beans>
  3. 测试
    存在的几个问题：
    1.src,源码包开始的路径，称为类路径的开始；
        所有源码包里面的东西都会被合并放在类路径里面；
        java:/bin/
        web:/web-inf/classes/
    2.导包commons-logging*.jar 依赖
    3.先导包再创建配置文件；
    4.Spring的容器接管了标志S的类；
    细节：
    1. ApplicationContext（IOC的容器接口）
        FileSystemXmlApplicationContext("F://ioc.xml");ioc容器的配置文件再磁盘路径下
    2. 组件的创建工作是容器完成的；
        容器中对象的创建再容器创建完成的时候已经创建好了
    3. 同一个组件在ioc容器中是单实例的，容器创建完成的时候已经创建好了
    4. 容器如果没有这个组件，报异常NoSuchBean....
    5. property会利用setter方法为javaBean的属性进行赋值
    6. javaBean的属性名是由什么决定的？getter/setter方法是属性名，去掉set然后小写首字母
        所有getter/serter都自动生成
    7.
    //从容器中获取组件
    @Test
    public void test(){
        //ApplicationCOntext:代表ioc容器
        //ClassPathXmlApplicationContext:当前应用的xml配置文件在ClassPath下
        //根据spring的配置文件得到ioc容器
        ApplicationCOntext ioc = new ClassPathXmlApplicationContext("ioc.xml");
        Person bean = ioc.getBean("person01");
    }
  ```
### 1.3 bean
- 根据bean的类型从ioc容器获取bean实例
- 通过构造器为bean的属性进行赋值
- 通过构造器为bean的属性进行赋值（index-type）
- 通过p命名空间为bean赋值
- 属性赋值null  
  ```
  <proerty name="lastname">
    <null/>
  </proerty>
  ```
- ref引用外部的值
- 引用内部的bean
- list属性赋值
- map属性赋值
  ```
  <proerty name="lastname">
    <map>
        <entry key="k1" value="1"></entry>
        <entry key="k1"><value>test</value></entry>
        <entry key="k2" ref=""></entry>
        <entry key="k3">
            <bean class="com.*.*">
            </bean>
        </entry>
    </map>
  </proerty>
  ```
- properties属性赋值
  ``` 
  <!-- 均为string类型 -->
  <proerty name="properties">
    <props key="name">root</props>
  </proerty>
  ```
- util名称空间创建集合类型的bean；方便别人引用
  ``` 
  <!-- 均为string类型 -->
  <uitl:map id="mymap">
    <entry key=""></entry>
    <list></list>
    <value></value>
    <ref bean="mymap"/>
  </uitl:map>
  <proerty name="maps" ref="mymap">
  ```
- 级联属性赋值  
  属性的属性
  ```
  <bean id="" class="">
    <property name="car" ref="car01">
    </property>
    <!-- 为属性的属性赋值 -->
    <property name="car.price" value="11"></property>
  </bean>
  ```
- 继承实现bean的重用
  ```
  <!-- 仅继承配置信息，class可不写 -->
  <bean id="" class="" parent="person05" abstract="true">
    <property name="name" value="李四"></property>
  </bean>
  ```
- bean的abstract属性仅允许被继承，不允许实例化
  ```
  <!-- abstract -->
  <bean id="" class="" parent="person05" abstract="true">
    <property name="name" value="李四"></property>
  </bean>
  ```
- bean之间的依赖
  ```
  默认按照配置的顺序创建
  depends-on="person,car"
  ```
- *bean的作用域，单实例和多实例
  ```
  bean默认为单实例
  scope属性：
    prototype:多实例
        1. 默认不会创建多实例bean
        2. 获取的时候创建bean
        3. 每次获取时创建对象
    singleton:单实例，默认
        1. 容器启动完成之前就创建好对象，保存在容器中
        2. 任何时候获取都是同一个实例
    request:web环境下，同一次请求创建一个bean实例（没用）
    session:web环境下，同一次会话创建一个bean实例（没用）
    <bean id="test" class="" scope=""></bean>
  ```
- *静态工厂方法创建bean,实例工厂方法创建的bean、FactoryBean
  ```
  bean的创建默认就是框架利用反射new出来的bean实例
  静态工厂：工厂本身不创建对象，通过静态方法调用，对象 = 工厂类.工厂方法名（）
  <bean id="" class="*.*Factory" factory-method="getAirPlan">
    <constructor-arg value="lisi">/<constructor>
  </bean>
  实例工厂：工厂本身需要创建对象
    AirPlane air = new AirPlane();

    <bean id="factory" class=""></bean>
    <!-- 未使用反射创建的 -->
    <bean id="" class="*.*Factory" factory-bean="Factory" factory-method="getAirPlan>
    <constructor-arg value="lisi">/<constructor>
  </bean>

  FactoryBean:
  ioc容器启动时不会创建实例，获取时创建

  1. 编写一个FactoryBean的实现类
    implements FactoryBean<Book>{
        @Override
        public Book getObject(){
            Book book = new ...;
            return book
        }
        @Override
        public Class<?> getObjectType(){
            retun book.class;
        }
        @Override
        pulic boolean isSingleton(){
            return false;
        }
    }
  2. 在spring配置文件中进行注册
  ```
- 创建带生命周期的bean
  ```
    单例：bean的生命周期 
      （容器开启）构造器--->初始化方法--->（容器关闭）销毁方法
    多实例：
      获取bean（构造器--->初始化方法）--->容器关闭不会调用bean的销毁方法
  ```
- 后置处理器
  ```
  （容器开启）构造器--->后置处理器before--->初始化方法--->后置处理器after--->（容器关闭）销毁方法
  多实例无销毁

    public class MyBeanPostProcess implements BeanPostProcessor

    xml注册 <bean id="process" class="com.testchn.bean.MyBeanPostProcess"></bean>

  ```
- 12.引用外部属性文件
  ```
  ioc.getBean(DataSource.class)
  <context:property-placeholder location="classpath:dbconfig.properties">
  <property name="" value="${username}"></property>
  <!-- username是spring的key中的一个关键字不能用 -->
  ```
- 源码和普通文件夹
- 基于xml的自动装配
  ```
  <bean autowire=""></bean>
  property:手动赋值
  自动赋值：
    autowire="default"：不自动装配
    byName：属性名作为ID找这个组件
    byType：属性类型作为查找依据
      找到多个时报错；
      没找到时null；
      如果属性为list，容器可以把所有的book封装为一个list
    constructor：根据构造器进行赋值
      1.先按照参数类型进行装配，没有装配null
      2.找到多个，参数名作为id继续配置；找不到则null
      3.不会报错
  ```
- SEL spring的表达式
  ```
  计算表达式：#{12*5}
  其它bean：#{book}
  其它bean的属性：#{book01.bookName}
  静态方法：#{T(全类名).静态方法名(1,2).静态方法名()}
  非静态方法：#{bean对象的ID.方法名}
  ```
- *注解创建DAO、Controller、Service层
  ```
  @Controller：控制器，servlet层
  @Service：业务逻辑 @Service("id")括号内可修改id
  @Repository：数据层
  @Component：不属于以上几层
  @Scope()：单例、多实例
  @Qualifier("bookService")：指定一个名作为id，让spring不要使用变量名作为id
  >注解可以随便加，Spring底层不会去校验你的这个组件，是否真的是一个dao层或servlet层；主要是给程序员看
  
  步骤：
  1.给要添加的组件标记舒适
  2.告诉Spring，自动扫描加了注解的组件
  <context:component-scan class="com.testchn"></context:component-scan>
  3.一定要导入aop包，支持注解模式
    1.组件的id为类名的小写
    2.组件的作用域默认是单例

  ```
- 指定扫描包（context:exclude-filter）
  ```
  一定要禁用默认的过滤规则use-default-filters="false"
  type="annotation"：指定配出规则；按照注解进行排除，标注了指定注解的组件不要
    expression=""：注解的全类名
  type="assignable"：指定某个具体的类
    expression=""：类的全类名
  type="aspectj"：后来aspectj表达式
  type="custom"：自定义一个TypeFilter；自己写代码绝对哪些 使用
  type="regex"：可以写正则表达式
  <context:component-scan base-package="com.*" use-default-filters="true">
    <context:exclude-filter type="annotation" expression="org.">
  </context:component-scan>
  ```
- *自动装配@Autowired
  ```
  @Qualifier("bookService")：指定一个名作为id，让spring不要使用变量名作为id
  @Autowired(required=false)
    required为false时未找到不报错，自动赋值为null
  private BookService bookService;
  原理：
    1. 先按照类型去容器找到对应的组件
      1.找到：则赋值
      2.没找到：抛异常
      3.找到多个：装配规则
        1.按照变量名作为为id继续匹配
          1.匹配上：装配
          2.未匹配上：无该类型报错
            原因：因为按照变量名作为id继续匹配
            使用@Qualifier("id")指定一个新id
              1.找到：装配
              2.未找到：报错
    发现Autowired标注的自动装配属性默认一定是装配上的;
      找到就装配；找不到就算了赋值为null；
  @Autowired用在方法上
    1.这个方法也会在bean创建的时候自动运行
    2.这个方法上的每一个参数都会自动注入值       

  @Autowired,@Resource,@InJect都是自动装配的意思
  @Autowired：最强大；Spring自己的注释
  @Resource：j2dd;java标准

  @Resource：扩展性强；如果切换成另外一个容器框架，Resource仍可使用，Autowired就不行

  Spring单元测试
  1.spring-test-***.jar
  2.@ContextConfiguration(locations="classpath:app*.xml")
  3.@RunWith()指定用哪种驱动进行单元测试，默认是junit
  ```
- *测试泛型依赖注入
  ```
  abstract public class BaseDao <T>{
    abstract public void save();
  }

  @Repository
  public class UserDao extends BaseDao<User>{
      public void saveBook(){
          System.out.println("保存用户");
      }
      @Override
      public void save(){
          System.out.println("保存用户");
      }
  }

  public class BaseService<T> {
    @Autowired
    private BaseDao<T> baseDao;
    public void save(){
        baseDao.save();
    }
  }
  UserService userService = ioc.getBean(UserService.class);
  userService.save();
  泛型依赖注入，注入一个组件的时候，他的泛型也是参考标准
  获取完整类型：
  bookService.getClass().getFenericSuperclass()
    BaseService<Book>
  ```
### 1.4 AOP
- 动态代理，加日志
  ```

  ```