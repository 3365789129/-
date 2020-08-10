# Default-warehouse
Mainly engaged in java development, welcome to exchange technology with me

今天主要进行了环境搭建

注意点：
1.老师用的是ecilipse封装的STS进行编译，而我使用的是idea，这是一个很大的不同之处
2.parent,util,reverse在一个目录，webui,component,entity在parent目录下
3.因为是在06下进行逆向工程，所以映射文件，接口，bean类在06下？？？？？
4.

学习流程：
1.创建工程：创建6个工程，1个父工程（project）01-parent，3个子工程（module，在parent下创建）02-webui,03-component,04-entity，2个独立工程（project）05-util,06-reverse
  parent管理jar包版本号，同时聚合三个子工程。02继承03,03继承04,05
2.创建数据库：在本地创建数据库project_crowd，创建t_admin表，作为管理员数据库表
3.逆向工程（MBG）：06中配置pom.xml，resources中配置generatorConfig.xml，安装两个maven插件，Goals中执行Mybatis-generator:generate
4.Spring整合mybatis：4.1 03中导入Spring和mybatis整合所需的依赖jar包。4.2 02resoureces中配置数据库连接信息文件jdbc.properties。 4.3 02resoureces中配置mybatis-config。xml配置文件。
  4.4 02resoureces中创建spring-persist-mybatis.xml配置文件开始整合。 4.5 spring-persist-mybatis.xml中引入jdbc.properties，并配置数据库连接

知识点：
1.查看maven管理项目的某个jar包：Maven中点此项目，工具栏找向上箭头，ctrl+F进行搜索，输入要寻找的jar包名，可以exclude排除（操作后，相应项目的pom.xml文件，相应jar包位置会出现exclusion关键字，里面放的即为排除的jar包）
2.
