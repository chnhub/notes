
- [SSM](#ssm)
  - [1.Spring](#1spring)
----
# SSM
## 1.Spring
- IOC（容器）  
　Struts2
　Hibernate
　Mybatis  
- AOP（面向切面编程） 声明式事务  
　Spring-JdbcTemplate  

IOC:(Inversion(反转) Of Control)：控制反转；  
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

DI：（Dependency Injection）依赖注入;  
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
