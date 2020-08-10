# Default-warehouse
Mainly engaged in java development, welcome to exchange technology with me

今天主要进行了环境搭建

注意点：
1.老师用的是ecilipse封装的STS进行编译，而我使用的是idea，这是一个很大的不同之处
2.parent,util,reverse在一个目录，webui,component,entity在parent目录下
3.因为是在06下进行逆向工程，所以映射文件，接口，bean类在06下？？？？？
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
5.搭建日志环境：

知识点：
1.查看maven管理项目的某个jar包：Maven中点此项目，工具栏找向上箭头，ctrl+F进行搜索，输入要寻找的jar包名，可以exclude排除（操作后，相应项目的pom.xml文件，相应jar包位置会出现exclusion关键字，里面放的即为排除的jar包）
2.@RunWith和@ContextConfiguration注解的使用：@RunWith是一个运行器，可通过此注解指定运行环境；@ContextConfiguration中的locations={classpath}可引入配置文件，需要测试哪个配置文件所管理的bean对象，则将配置文件的相对路径引入即可。
