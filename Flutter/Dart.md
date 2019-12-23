# Dart语言

[Dart中文网](https://www.dartcn.com/)

强类型语言

### 开发适用场景

1、Web开发

2、跨平台应用开发 （Flutter）

3、脚本或服务器开发


### DartPad

[DartPad](https://dartpad.cn/)

在浏览器上编辑和运行Dart语言，快速熟悉Dart利器

### 语法

1、变量

    var a = 15;
    a = 'xx' //异常

    var b; //b未被初始化，默认为null
    b = 15; //正确
    b = 'xx'; //正确


2、常量

    可以使用final或const来修饰一个常量


3、内置类型

    （1）Number 数值型
        |-- num
        |-- int
        |-- double
        num声明的变量加入的是int型，还可以被改成double型，但是，反过来int声明的变量不能再赋值成double型。

        操作
        |-- 运算符 +、-、*、/、～/、%   （～/表示的是返回一个整数值的除法）
        |-- 常用属性 isNaN -- 是否为非数字、isEven -- 是否为偶数、isOdd -- 是否为奇数

            var a = 0 / 0; //这里不会报错，在java中是会报错的
            print(a.isNaN);

        |-- 常用方法  round(), floot(), ceil(), toInt(), toDouble(), abs()
            var c = 1.23;
            c.toInt();

    （2）String 字符串型

        字符串创建
            |-- 使用单引号、双引号创建字符串
            |-- 使用三个引号或双引号创建多行字符串
            |-- 使用r创建原始raw字符串

        字符串拼接：
            String a1 = 'ha' 'ha';
            Strig a2 = "ha" "ha";
            String a3 = 'ha' + 'ha';
            String a4 = "ha" + "ha";

        插值表达式： ${expression}
            double a = 100.0;
            print("a is $a");

    （3）Boolean 布尔型
        使用bool来定义

    （4）List 列表
        创建list
            var list = [1,2,3];
            var list2 = new List(); //通过构造方式创建list
            var list3 = const[1,2,3]; //创建一个不可变的list

        访问list
            list[0] = 3;//给第一个元素赋值为3

    （5）Map 键值对

        表示以键值对的方式存储，键和值都可以是任何类型的对象，每个键只能出现一次
        创建Map：
            Map game = {"name" : "swith", "company" : "任天堂"};  //直接声明方式创建一个Map
            Map game = const{"name" : "swith", "company" : "任天堂"};  //直接声明方式创建一个不可改变的Map

            //构造方式先声明，然后再赋值
            Map game = new Map();
            game['name'] = 'switch';
            game['company'] = '任天堂';

            //修改内容
            game['name'] = 'xxx'; //赋值修改
            game.remove('name'); //移除掉key为name的值
            game.clear(); //清空整个map集合

    （6）Runes 符号字符

    （7）Symbols 标识符

4、Dart特有的一些运算符

    三目运算符：
        expr1 ?? expre2 //如果expr1非空，则返回其值，否则返回expre2的值

    ~/除法：
        返回一个整数结果（取商）

    级联操作符：

    as、is、is!运算符

5、异常捕获

    抛出异常：
        throw Exception('我是异常');

    捕获异常：
        try {

        } on AuthorizationExceoption catch (e) {
            //捕获特定类型的异常
        } on Exception {
            //捕获所有异常
        } catch (e) {

        } finally {

        }

6、函数

    可选参数：以{param1, param2,...}指定命名参数
    void userSettings({int age, String name}) {
        //...
    }
    对于上面的函数，可以传递age、name这两个参数，或者其中一个参数也可以，甚至不传也可以的

    必传参数：以@required进行装饰
    void userSettings({@required int age, String name}) {
        //...
    }

    可选的位置参数：

    默认参数：
    void userSettings({int age = 21, String name = '小明'}) {
        //...
    }

    函数作为参数传递：
    void printItem(String item) {
        print(item);
    }
    var users = ['小明', '小王', '小张'];
    users.forEach(printItem);

    函数做为变量：
    var say = (name) {
        print(name);
    };
    say('过年了');

    匿名函数：
    => expr 等同于{ return expr; }

7、异步编程

    Future

    async和await

8、继承、接口实现和混合

    （1）继承（extends）。首先，明确一点，Flutter中的继承也是单继承。Flutter
    继承一个类之后，子类可以通过@override来重写超类（父类）中的方法，
    也可以通过super来调用超类中的方法。需要说明的是，构造函数不能被继承。
    另外，由于Flutter没有Java中的公有和私有访问修饰符，因此，可以直接访问超类中的所有变量和方法。


    （2）接口实现（implements）。Flutter是没有接口（interface）关键字的，
    但是Flutter中的每个类都是一个隐式的接口，这个接口包含类里的所有成
    员变量和定义的方法。当类被当作接口使用时，类中的方法就是接口中的方法，
    它需要在子类里重新被实现。在子类实现的时候要加@override

    （3）混合是一个可以把自己的方法提供给其他类使用，但却不需要成为其他
    类的父类的类，它以非继承的方式来复用类中的代码

    执行相同方法时的优先级：
        类本身的-》混合-》继承-》接口

9、泛型




