# Default-warehouse
Mainly engaged in java development, welcome to exchange technology with me

今天主要进行了环境搭建：创建工程，创建数据库，逆向工程，Spring与mybatis整合，搭建日志环境，声明式事务管理

注意点：
1.老师用的是ecilipse封装的STS进行编译，而我使用的是idea，这是一个很大的不同之处
2.parent,util,reverse在一个目录，webui,component,entity在parent目录下
3.因为是在06下进行逆向工程，所以映射文件AdminMapper.xml，接口AdminInterface，bean类Admin在06下,AdminMapper.xml放在02，AdminInterface和Admin放在03,04？？？？
4.01及其子工程中测试类的jar包需要把scope范围去掉，否则@RunWith和@ContextConfiguration注解将无法生效。

学习流程：
1.创建工程：创建6个工程，1个父工程（project）01-parent，3个子工程（module，在parent下创建）02-webui,03-component,04-entity，2个独立工程（project）05-util,06-reverse
  parent管理jar包版本号，同时聚合三个子工程。02继承03,03继承04,05
2.创建数据库：在本地创建数据库project_crowd，创建t_admin表，作为管理员数据库表
3.逆向工程（MBG）：06中配置pom.xml，resources中配置generatorConfig.xml，安装两个maven插件，Goals中执行Mybatis-generator:generate
4.Spring整合mybatis：4.1 03中导入Spring和mybatis整合所需的依赖jar包。
                    4.2 02resoureces中配置数据库连接信息文件jdbc.properties。 
                    4.3 02resoureces中配置mybatis-config。xml配置文件。
                    4.4 02resoureces中创建spring-persist-mybatis.xml配置文件开始整合。 
                    4.5 spring-persist-mybatis.xml中引入jdbc.properties，并配置数据库连接
                    4.6 02中创建test类进行测试，使用@RunWith和@ContextConfiguration注解
                    4.7 spring-persist-mybatis.xml配置SqlSessionFactoryBean整合mybatis，里面指定mybatis配置文件，指定Mapper.xml配置文件，装配数据库。
                    4.8 spring-persist-mybatis.xml中配置MapperScannerConfigure来扫描Mapper接口所在的包
                    4.9 创建的test类中进行测试，自动装配Mapper接口，调用接口方法，通过映射文件执行sql语句，观察数据库变化
5.搭建日志环境： 5.1 排除Spring中的common-logging日志类jar包，用jcl-over-slf4j.jar包转换为slf4j.jar包管理日志，排除方法见知识点1
               5.2 02resoureces中创建logback.xml配置文件，指定文件输出位置，文件输出格式，全局日志级别，局部日志级别
               5.3 在test类中创建Logger类对象进行日志输入测试。
6.声明式事务管理： 6.1 创建spring-persist-tx.xml配置文件用来进行事务配置，配置自动扫描的包，把类交给IOC管理
                 6.2 配置事务管理器DataSourceTransactionManager，在其内配置数据源
                 6.3 配置事务切面AOP，写入切入点表达式execution，用advisor将切入点表达式和事务通知联系起来
                 6.4 配置事务通知transaction-manager，在其内配置事务属性tx:attributes
                 6.5 事务属性，查询方法，readonly；增删改，propagation设置为REQUIRES_NEW,rollback-for设置属性让编译及运行时的异常都回滚
                 6.6 在test类中自动配置AdminService，创建方法进行测试，观察控制台打印的信息

知识点：
1.查看maven管理项目的某个jar包：Maven中点此项目，工具栏找向上箭头，ctrl+F进行搜索，输入要寻找的jar包名，可以exclude排除（操作后，相应项目的pom.xml文件，相应jar包位置会出现exclusion关键字，里面放的即为排除的jar包）
2.@RunWith和@ContextConfiguration注解的使用：@RunWith是一个运行器，可通过此注解指定运行环境；@ContextConfiguration中的locations={classpath}可引入配置文件，需要测试哪个配置文件所管理的bean对象，则将配置文件的相对路径引入即可。
3.System.out.println本质是流输出，一旦用多了很耗内存，因此输出信息用日志，日志的级别：DEBUG<INFO<WARN<ERROE，若设置日志级别为INFO,则不输出DEBUG的信息
4.事务通知tx:advice中的事务属性tx:attributes中的tx:method，里面的propagation属性建议赋值为REQUIRES_NEW，这样此tx:method所管理的方法算是一个单独的事务，若赋值为REQUIRED，则事务会受到当前线程中的事物的影响（线程事务回滚，则此方法中的事务也会被迫回滚）
5.自动装配的理解:5.1 首先spring中开启注解扫描
               5.2.在接口的实现类上加上注解，接口不加
               5.3.类中定义接口属性，其上加@Autowired注解，实现自动装配
               注：因为接口和实现类类型一样，所以虽然定义的是接口，但最终会装配到实现类上（类比多态的向上转型）
6.向上转型: 父类引用指向子类对象Animal a = new Cat()，a调用子类重写父类的方法（这一步可以减少代码量，是向上转型的优点，无需每个子类重写调用方法），子类特有的方法会丢失。
  向下转型: 必须在向上转型之后进行，Animal a = new Cat()，Cat c=(Cat) a，有危险，Dog引用a则会报错，c可调用子类特有的方法
