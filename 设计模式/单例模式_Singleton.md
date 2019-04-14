# 单例模式——Singleton

#### 定义：
保证一个类只有一个实例，并且提供一个全局方法来获取该实例。

#### 特征：
- 私有化的静态实例
- 私有化的构造方法
- 返回实例的静态公共方法

#### 例子：

1. 饿汉式单例
```
public class Singleton(){
    private static Singleton instance = new Singleton();
    
    private Singleton(){};
    
    public static Singleton getInstance(){
        return instance;
    }
}
```
2. 懒汉式单例
```
public class Singleton(){
    private static Singleton instance = null;
    
    private Singleton(){};
    
    public static synchronized Singleton getInstance(){
        if(instance == null){
            instance = new Singleton();
        }
        return instance;
    }
}
```

#### 优点
- 某些类创建频繁，或者比较大，使用单例可提高性能，节省内存
- 避免共享资源多重占用
- 方便的全局访问

#### 补充
- 
