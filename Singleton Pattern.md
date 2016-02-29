* 单例的一些故事
```
public class Singleton {

    private static Singleton obj = null;
    private static final Object syncRoot = new Object();

    private Singleton() {
    }

    public static Singleton getInstance() {
        synchronized(syncRoot){
            if (obj == null) {
                obj = new Singleton();
            }
        }
        return obj;
    }
}
```

```
public class Singleton {

    private Singleton() {
    }

    private static class HolderClass {
        private final static Singleton instance = new Singleton();
    }

    public static Singleton getInstance() {
        return HolderClass.instance;
    }
}
```

```
    public class InstanceHolder{
      private static Instance ins = null;
      private Instance(){}

      public static Instance getInstance(){
        if(ins == null){
          synchronized(InstanceHolder.class){
            if(ins == null){
               ins = new Instance();
            }
          //有可能在锁释放之前，刷新了主内存数据，new时候的指令重排问题
          }
        }
        return ins;
      }
    }
    //解决方案 java5 之后，被volatile修饰的对象，将禁止该对象上的读写指令重排序
```
[IBM-Java单例对象同步问题探讨](https://www.ibm.com/developerworks/cn/java/l-singleton/)<br/>
[博客-DCL失效问题的探讨 ](http://blog.itpub.net/28912557/viewspace-762047/)<br/>
[补充-Java 线程/内存模型的缺陷和增强](http://www.blogjava.net/weidagang2046/articles/3494.html)
