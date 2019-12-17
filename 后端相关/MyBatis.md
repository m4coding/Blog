# MyBatis

## MyBatis代码生成器

xml标签说明

    mybatis-generator-config_1_0.dtd 定义了标签属性和其用法
    generatorConfiguration  根标签节点  必备元素

    子标签：
        1、properties标签：用来指定外部的属性元素，最多可以配置1个，也可以不配置。
            properties标签包含resource和url两个属性，只能使用其中一个属性来指定，同时出现则会报错。
            （1）resource：指定classpath下的属性文件，类似com/myproject/generatorConfig.properties这样的属性值。
            （2）url：指定文件系统上的特定位置，例如file：///C：/myfolder/generatorConfig.properties。

       2、classPathEntry标签：这个标签可以配置多个，也可以不配置。
            classPathEntry标签最常见的用法是通过属性location指定驱动的路径
            例如：
                <classPathEntry location=＂E:\mysql\mysql-connector-java-5.1.29.jar＂/>

       3、context标签：该标签至少配置1个，可以配置多个
            context标签用于指定生成一组对象的环境。
            例如指定要连接的数据库，要生成对象的类型和要处理的数据库中的表。运行MBG的时候还可以指定要运行的context。
            属性：
                （1）id
                    必选属性，用于唯一确定该标签的

                （2）defaultModelType
                    定义MBG如何来生成实体类
                    可选值：
                        * conditional 默认值， 如果一个表的主键只有一个字段，那么不会为该字段生成单独的实体类，而是会将该字段合并到基本实体类中。
                        * flat  该模型只为每张表生成一个实体类。这个实体类包含表中的所有字段。这种模型最简单，推荐使用。
                        * hierarchical ：如果表有主键，那么该模型会产生一个单独的主键实体类，如果表还有BLOB字段，则会为表生成一个包含所有BLOB字段的单独的实体类，
                                    然后为所有其他的字段另外生成一个单独的实体类。MBG会在所有生成的实体类之间维护一个继承关系。

                （3）targetRuntime
                    用于指定生成的代码的运行时环境
                    可选值：
                        * MyBatis3 默认值
                        * MyBatis3Simple 这种情况不会生成与Example相关的方法

                （4）introspectedColumnImpl：
                    该参数可以指定扩展org.mybatis.generator.api.Introspected Column类的实现类。

            子标签：
                （1）property（0个或多个）
                        子属性：
                            1、autoDelimitKeywords
                            2、beginningDelimiter
                            3、endingDelimiter
                            4、javaFileEncoding
                            5、javaFormatter
                            6、xmlFormatter
                （2）plugin（0个或多个）
                        标签用来定义一个插件，用于扩展或修改通过 MBG 生成的代码
                （3）commentGenerator（0个或1个）
                        该标签用来配置如何生成注释信息
                （4）jdbcConnection（1个）
                        用于指定MBG要连接的数据库信息，该标签必选，并且只能有一个
                        子属性：
                            1、driverClass （必选属性）访问数据库的JDBC驱动程序的完全限定类名
                            2、connectionURL （必选属性） 访问数据库的JDBC连接URL
                            3、userId （可选属性） 访问数据库的用户ID
                            4、password （可选属性） 访问数据库的密码
                （5）javaTypeResolver（0个或1个）
                        该标签的配置用来指定JDBC类型和Java类型如何转换，最多可以配置一个

                （6）javaModelGenerator（1个）
                        该标签用来控制生成的实体类，根据context标签中配置的defaultModelType属性值的不同，一个表可能会对应生成多个不同的实体类。
                        该标签必须配置一个，并且最多配置一个。
                        子属性：
                            1、targetPackage （必选属性） 生成实体类存放的包名。一般就是放在该包下，实际还会受到其他配置的影响
                            2、targetProject （必选属性） 指定目标项目路径，可以使用相对路径或绝对路径
                        子标签property属性：
                            1、constructorBased 该属性只对MyBatis3有效，如果为true就会使用构造方法入参，如果为false就会使用setter方式。默认为false
                            2、enableSubPackages 如果为true，MBG会根据catalog和schema来生成子包。如果为false就会直接使用targetPackage属性。默认为false
                            3、immutable
                            4、rootClass 设置所有实体类的基类
                            5、trimStrings 判断是否对数据库查询结果进行trim操作，默认值为false
                （7）sqlMapGenerator（0个或1个）
                        该标签用于配置SQL映射生成器（Mapper.xml文件）的属性，该标签可选，最多配置一个
                （8）javaClientGenerator（0个或1个）
                        该标签用于配置Java客户端生成器（Mapper 接口）的属性，该标签可选，最多配置一个。如果不配置该标签，就不会生成Mapper接口。
                （9）table（1个或多个）
                        table是最重要的一个标签，该标签用于配置需要通过内省数据库的表，
                        只有在table中配置过的表，才能经过上述其他配置生成最终的代码，该标签至少要配置一个，可以配置多个。
                        子属性：
                            1、tableName
                            2、schema
                            3、catalog
                            4、alias
                            5、domainObjectName
                            6、enableXXX
                            7、selectByPrimaryKeyQueryId
                            8、selectByExampleQueryId
                            9、modelType
                            10、escapeWildcards
                            11、delimitIdentifiers
                            12、delimitAllColumns
                            13、constructorBased
                            14、ignoreQualifiersAtRuntime
                            15、immutable
                            16、modelOnly
                            17、rootClass
                            18、rootInterface
                            19、runtimeCatalog
                            20、runtimeSchema
                            21、runtimeTableName
                            22、selectAllOrderByClause
                            23、useActualColumnNames
                            24、useColumnIndexes
                            25、useCompoundPropertyNames
                        子标签：
                            1、property
                            2、generatedKey（0个或1个）
                            3、columnRenamingRule（0个或1个）
                            4、columnOverride（0个或多个）
                            5、ignoreColumn（0个或多个）
                                该标签可以用来屏蔽不需要生成的列，该标签可选，可以配置多个。

## MyBatis插件相关开发

    //实现拦截的几个方法说明
    public interface Interceptor {

      Object intercept(Invocation invocation) throws Throwable;

      Object plugin(Object target);

      void setProperties(Properties properties);

    }

    1、void setProperties(Properties properties);
        此方法用于给插件传递配置参数的

    2、Object plugin(Object target);
        Plugin.wrap 方法会自动判断拦截器的签名和被拦截对象的接口是否匹配，只有匹配的情况下才会使用动态代理拦截目标对象，因此在上面的实现方法中不必做额外的逻辑判断。

    3、Object intercept(Invocation invocation) throws Throwable;
        具体的拦截操作在这个方法进行处理

当配置多个拦截器时，MyBatis 会遍历所有拦截器，按顺序执行拦截器的 plugin 方法，被拦截的对象就会被层层代理。

在执行拦截对象的方法时，会一层层地调用拦截器，拦截器通过invocation.proceed（）调用下一层的方法，直到真正的方法被执行。

方法执行的结果会从最里面开始向外一层层返回，所以如果存在按顺序配置的A、B、C三个签名相同的拦截器，

MyBaits会按照C＞B＞A＞target.proceed（）＞A＞B＞C的顺序执行。如果A、B、C签名不同，就会按照MyBatis拦截对象的逻辑执行。

## 一些坑

1、关于insertSelective()返回值，如果插入成功则返回1，失败返回0；
对于updateByPrimaryKeySelective()的返回值，成功更新几条返回则返回相应的条数，比如更新了3条，返回值为3，如果更新失败返回0。

然后insertSelective插入成功后，获取键值是保存在对应Bean的字段中的。


2、使用生成代码插件时，重复编译生成代码，mapper.xml文件是直接追加原来文件后面的，而不是直接覆盖的问题处理

[MyBatis Generator使用过程中踩过的一个坑](https://www.jianshu.com/p/65daea588077)


3、Generator生成代码时，tinyint自动转换为byte的处理方法：可以继承JavaTypeResolverDefaultImpl重新设置为Integer即可

[mybatis-generator代码生成（支持自定义类型转换）](https://blog.csdn.net/lichuangcsdn/article/details/80873737)


4、使用PageHelper插件时的一些坑：

    （1） Page page = PageHelper.startPage(pageNum, pageSize);
        page记录的是最接近的那次select的结果，也是有效于最近那次的

     同时通过PageHelper.getLocalPage获取的page有时会为空的，因为被clear掉了。。

     在使用PageHelper时，设置startPage、查询数据、转化为pageInfo必须是连贯的三步，中间不能有任何其他处理否则会导致结果不可预期