
#### 1.Java跨平台原理
由于不同平台，指令也集不同，Java通过虚拟机对不同平台进行了适配，对外提供统一的编程接口。
    
#### 2.Java基本数据类型（8种）
| 数据类型 | 大小(bit) | 范围            | 默认值 |
| -------- | --------- | --------------- | ------ |
| byte     | 8         | -128-127        | 0      |
| short    | 16        | -2^15 - 2^15-1  | 0      |
| int      | 32        | ...             | 0      |
| lont     | 64        | ...             | 0      |
| float    | 32        | ...             | 0.0f   |
| double   | 64        | ...             | 0.0d   |
| char     | 16        | \u0000 - u\ffff | \u0000 |
| boolean  | 1         | true/false      | false  |

#### 3.面向对象的特征
- 抽象：将一类对象的共同特征总结出来构造类的过程，包括数据抽象和行为抽象两方面。抽象只关注对象有哪些属性和行为，并不关注这些行为的细节是什么
- 封装：把数据和操作数据的方法绑定起来，对数据的访问只能通过已定义的接口。方法就是对实现细节的一种封装；封装就是隐藏一切可隐藏的东西，只向外界提供最简单的接口（如普通洗衣机和全自动洗衣机的差别）
- 继承：从已有类得到继承信息创建新类的过程
- 多态：多态性是指允许不同子类型的对象对同一消息作出不同的响应。简单的说就是用同样的对象引用调用同样的方法但是做了不同的事情。方法重载（overload）实现的是编译时的多态性，而方法重写（override）实现的是运行时的多态性
    
#### 4.包装类型
byte - Byte，
short - Short， 
int - Integer，
long - Long，
float - Float， 
double - Double， 
char - Character， 
boolean - Boolean  
装箱：基本数据类型 -> 包装类型  
拆箱：包装类型 -> 基本数据类型  
手动拆/装箱：Integer.intValue()/Integer.valueOf()  

Q：为什么有包装类型？  
A：java是面向对象的语言，而基本数据类型不具备面向对象的特征 

**注意：对于Integer类型，如果整型字面量的值在-128到127之间，那么不会new新的Integer对象，而是直接引用常量池中的Integer对象**

#### 5.“==”和equals方法
`==`：对于基本数据类型，比较的是值；非基本数据类型，比较的是引用地址  
`equals`：由比较对象中重写的`equals`方法决定，若没有重写，则使用`Object`中的`equals`方法（`Object`中equals方法直接调用==）

**建议：对于非基本数据类型，全部使用`equals`方法**

#### 6.String，StringBuilder，StringBuffer
- String是内容不可变对象（底层实现是不可变字符数组`final char[]`），而StringBuilder，StringBuffer是可变的（底层实现是可变字符数组，无final修饰） 
- StringBuilder非线程安全，效率比StringBuffer高，StringBuffer是线程安全的

> String str = new String("hello");

上面的语句中变量str放在栈上，用new创建出来的字符串对象放在堆上，而"hello"这个字面量是放在方法区的。

Q：判断下面代码结果
```
String s1 = "ab";
String s2 = "cd";
String s3 = "ab" + "cd";
String s4 = s1 + s2;
String s5 = new String("ab") + new String("cd");
String s6 = new String("ab") + "cd";
String s7 = s1 + "cd";
String s8 = new String("ab") + s2;
String s = "abcd";
System.out.println(s3 == s); // true
System.out.println(s4 == s); // false
System.out.println(s5 == s); // false
System.out.println(s6 == s); // false
System.out.println(s7 == s); // false
System.out.println(s8 == s); // false
```
A：“+”号两边只要有非方法区中的字符串对象那么必然会在内存中创建新对象

#### 7.Java中的集合
Java中的集合可分为value和key-value两种  
- key：List（有序，可重复），Set（无序，不可重复）
- key-value：Map

ArrayList与LinkedList  
ArrayList：底层实现->数组，查询特定元素块，但插入、删除、修改比较慢
> 数组在内存中是一块连续的空间，增删改需要移动内存

LinkedList：底层实现->链表，查询较慢，但是插入、删除、修改较快
> 链表在内存中不要求连续，所以查询需要一个个找，而增删改只需要改变引用地址

Map的底层实现：1.8以前是数组+链表，1.8是数组+链表+红黑树

HashMap与HashTable及ConcurrentHashMap  
HashMap：线程不安全，null可以作为key  
HashTable：线程安全，key不能为null  
ConcurrentHashMap：线程安全且效率较高。原理是把整个Map分为N个Segment（类似HashTable）

#### 8.实现文件的拷贝
````
public static fileCopy(String source, String target) throws IOException {
    try (InputStream in = new FileInputStream(source)) {
        try (OutPutStream out = new FileOutPutStream(target)) {
            byte[] buffer = new byte[4096];
            int r;
            while ((r = in.read(buffer)) != -1) {
                out.write(buffer, 0, r);
            }
        }
    }
}
````

#### 9.线程的创建，启动，区分，线程池，并发库
线程实现：
- 继承Thread类（重写run方法）
    ```
    Thread thread = new Thread(一个继承了Thread的类对象);
    thread.start();
    ```
- 实现Runnable接口（重写run方法）
    ```
    Thread thread = new Thread(一个实现了Runnable接口的类对象);
    thread.start();
    ```
- 实现Callable接口（Java5）

    ````
    import java.util.ArrayList;
    import java.util.List;
    import java.util.concurrent.Callable;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    import java.util.concurrent.Future;
    
    class MyTask implements Callable<Integer> {
        private int upperBounds;
        public MyTask(int upperBounds) {
            this.upperBounds = upperBounds;
        }
        @Override
        public Integer call() throws Exception {
            int sum = 0; 
            for(int i = 1; i <= upperBounds; i++) {
                sum += i;
            }
            return sum;
        }
    }
    
    class Test {
        public static void main(String[] args) throws Exception {
            List<Future<Integer>> list = new ArrayList<>();
            ExecutorService service = Executors.newFixedThreadPool(10);
            for(int i = 0; i < 10; i++) {
                list.add(service.submit(new MyTask((int) (Math.random() * 100))));
            }
            int sum = 0;
            for(Future<Integer> future : list) {
                // while(!future.isDone()) ;
                sum += future.get();
            }
            System.out.println(sum);
        }
    }
    ````
多线程的并发库：  
Java通过Executors工具类提供了4种创建线程池的静态方法分别是：
- newSingleThreadExecutors
- newFixedThreadPool
- newCachedThreadPool
- newScheduledThreadPool

**补充：在spring框架中，可以通过spring提供的ThreadPoolTaskExecutor()创建线程池**
Q：为什么使用线程池？
A：。。。。

#### 10. 讲一下设计模式
设计模式：无数人实践总结出来的一套能反复使用的、解决问题发设计方法。  
常用设计模式：
- 单例模式：（Spring的bean单例是通过ConcurrentHashMap创建一个单例注册表来实现的）
- 工厂模式：
- 适配器模式：
 
#### 11. HTTP get和post区别
- get、post都是http请求方式，此外还有 put、delete请求
- get请求的参数直接拼接到url中，post是放在请求体内
- 由于url长度有限，所以get传输数据大小有限
- get一般用于获取数据，post一般用于修改数据

#### 12. 说下下Servlet的生命周期
加载Servlet的class文件，创建servlet实例，执行servlet的init方法完成初始化，响应请求时调用servlet的service方法，servle容器关闭时，调用destroy方法

#### 13. forward()与redirect()区别
- forward：服务器端跳转，用户请求地址不变；spring中的实现`return "redirect:/toUrl ";
- redirect:客户端跳转，用户请求地址会发生改变，用户会再发一个请求；spring中的实现`return "forward:/toUrl ";`

#### 14：jsp内置对象
- request 用户端请求，包含请求参数
- pageContext 网页的属性
- session 会话相关的
- application

上面的是四大作用域，可以利用jstl从中获取值

#### 15. session与cookie区别？
- session和cookie都是会话跟踪，session是保存在服务端的，且依赖于cookie中存放的sessionid，
cookie是存放在客户端的
- cookie大小、数量有限制，一般不能超过4k

#### 16. 介绍一下MVC设计模式
- Model：数据模型，如VO、JavaBean
- View：视图，如html，jsp，freemarker
- Controller：控制器 如Servlet、Controller

### 2.数据库相关
#### 1.数据库分类
- 关系型数据库：mysql、oracle、sqlserver
- 非关系型：redis、mongodb、memcached

#### 2.数据库事务
> ACID（原一隔持）
- A 原子性 事务内操作不可分割，要么全部成功，要么全部失败，任何一个失败会导致整个事务内的操作失败
- C 一致性 事务结束后系统状态是一致的
- I 隔离性 并发执行的事务不会相互影响
- D 持久性 事务完成后，改动会被持久化

#### 3.MySql：  
默认最大连接数 **100**
分页 `LIMIT [offset,]size` 从offset位置开始取size条数据，或者取前size条数据  
> 如果问Oracle数据库分页，答：Oracle没怎么用过，好像实现比较复杂，是通过三层的嵌套查询实现的

#### 4.数据库触发器：  
触发器，当满足特定条件时，触发预定的操作  
```
delimiter |
create trigger dosomething_trigger after insert on xx_table for each row begin
    -> update xx_table set create_time = now() where id = NEW.id;
    -> end;
    -> |
delimiter ;
```

#### 4.数据库存储过程：  
一段预编译好的sql语句，提高了执行效率；可实现较为复杂的查询
```
create procedure dosome_procedure(_id long, _name varchar(20),out _age int)
begin
    insert into student value(_id, _name, null);
    select max(age) age from student

call dosome_procedure(100, 'Bob', @age);
select @age;
```
#### 5.jdbc执行sql的过程：  
1. 加载数据库驱动
2. 创建数据库连接
3. 准备sql语句，设置参数
> 调用存储过程的sql `call dosome_procedure(?, ?, ?)`
4. 执行sql语句
5. 释放数据库连接

#### 6.数据库连接池的作用：。。。。

#### 7.jdbc中PreparedStatement和Statement比较
PreparedStatement 是预编译的，比Statement速度快，sql可维护性、可读性较好，能防止sql注入攻击

### 3.前端相关
#### 1. HTML、CSS、JavaScript在前端中的作用？
- HTML 一种超文本标记语言，定义网页结构
- CSS 层叠样式表，美化页面
- Javascript 实现交互逻辑

#### 2.介绍以下Ajax
Ajax是异步的JavaScript与xml  
他可以利用XmlHttpRequest对象  
在不刷新页面的情况下，与服务器交换数据  
实现对页面的局部刷新

#### 3.js和jQuery
> jQuery是一个js框架，封装了js的属性和方法，对js进行了增强，使用更加便利

#### 4.说一下HTML5
html5是最新版本的html，在html4的基础上，增加了一些标签  
canvas-画布、audio、video、web高级存储等

#### 5.说以下CSS3
#### 6.bootstrap是什么？
bootstrap是一个移动设备优先的UI框架，可以在不写css、js代码的情况下实现较为好看的页面（模态框、栅格布局、样式库）

### 4.框架相关
#### 1.说一下SpringMVC执行流程
。。。

#### 2.什么是IOC（或DI）？什么是AOP

#### 3.Spring事务传播行为
Spring的事务传播行为（Propagation）共有7种
- Propagation.REQUIRED 默认传播行为，保持原有的事务
- Propagation.REQUIRES_NEW 挂起原有事务，开启新事务
- Propagation.SUPPORTS 有则使用，无则非事务执行
- Propagation.NOT_SUPPORT 挂起事务，以非事务执行
- Propagation.NEVER 只能非事务执行，否则异常
- Propagation.NESTED 有就嵌套事务执行，无则开启
- 。。。。

#### 4.Spring事务的隔离级别
Spring支持4种事务隔离级别
- Isolation.REPEATABLE_READ 可重复读，MySQL默认级别
- Isolation.READ_COMMITTED 读已提交，Oracle默认级别

| 事务级别 | oracle | mysql |
| -------- | ------ | ----- |
| 读未提交 | ✗      | ✓     |
| 读已提交 | ✓      | ✓     |
| 可重复读 | ✗      | ✓     |
| 序列化   | ✓      | ✓     |

#### 5.什么是ORM
对象关系映射。。。。

#### 6.什么是Activiti？
Activiti是一个**业务流程管理**和**工作流**系统，核心是BPMN2流程引擎。与Spring集成使用。  
主要用于OA中，把线下流程放到线上