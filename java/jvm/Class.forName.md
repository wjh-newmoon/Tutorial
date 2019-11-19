`Class.forName() `的作用

## 作用

1. 装载一个类并对其进行实例化操作。
2. 装载过程中使用的类加载器是当前类



## 类比描述(`classLoader.loadClass() `)



`Class.forName(String className) `使用装载当前类的类装载器来装载制定的类，因为`class.forName(String name)` 方法内部调用了 `forName0(className, true, ClassLoader.getClassLoader(Reflection.getCallerClass()))`方法。



`classLoader.loadClass(String className, Boolean resolve);`需要手动制定装载器的实例。



`Class.forName(className);`装载的类已经实例化，`classLoader.loadClass(); `则只是将信息装载给 JVM. 

在JDBC中 `Class.forName(“com.mysql.jdbc.Driver”)`，如果换成`getClass().getClassLoader().loadClass(“com.mysql.jdbc.Driver”)`，就不可以，因为它只是想JVM装载了Driver的类信息，但是没有实例化，也就不能执行相应的操作，因为Driver是需要被初始化才能被使用的。

---

参考: <https://blog.csdn.net/syilt/article/details/90706332>