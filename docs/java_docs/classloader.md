# ClassLoader

--- 

## 一、AppClassLoader

`AppClassLoader`(应用类加载器)，又称系统类加载器。负责在`JVM`启动的时候，加载来自

- `jvm`启动时的`classpath`
- `java.class.path`系统属性
- `CLASSPATH`系统属性

中的`jar`包和类路径。

```java
// 用于输出当前系统类加载的classloader
ClassLoader.getSystemClassLoader();

// 
String classPath = System.getProperty("java.class.path");
for (String path : classPath.split(";")) {
  System.out.println(path);
}
```

---

## 二、ExtClassLoader

`ExtClassLoader`(扩展类加载器)。主要负责加载`JVM`的扩展类库。

主要加载的位置；

- `$JAVA_HOME/jre/lib/ext/`
- `java.ext.dirs`

放入上述的目录当中的`jar`包对于`AppClassLoader`都是可见的（双亲委派机制）。

```java
String extDirs = System.getProperty("java.ext.dirs");
for (String path : extDirs.split(";")) {
  System.out.println(path);
}
```

---

## 三、BootstrapClassLoader

`BootstrapClassLoader`(启动类加载器)。是`JVM`类加载模型中最顶层的类加载器，负责加载`JDK`中的核心类库。

- `rt.jar`
- `resources.jar`
- `charsets.jar`
- ...

```java
URL[] urLs = sun.misc.Launcher.getBootstrapClassPath().getURLs();
for (URL url : urLs) {
  System.out.println(url.toExternalForm());
}
```

!> `Bootstrap ClassLoader`是由`C/C++`编写的，它本身是虚拟机的一部分，所以它并不是一个`JAVA`类，也就是无法在`java`代码中获取它的引用，`JVM`启动时通过`Bootstrap`类加载器加载`rt.jar`等核心`jar`包中的`class`文件，之前的`int.class,String.class`都是由它加载。然后呢，我们前面已经分析了，`JVM`初始化`sun.misc.Launcher`并创建`Extension ClassLoader`和`AppClassLoader`实例。并将`ExtClassLoader`设置为`AppClassLoader`的父加载器。`Bootstrap`没有父加载器，但是它却可以作用一个`ClassLoader`的父加载器。比如`ExtClassLoader`。这也可以解释之前通过`ExtClassLoader`的`getParent`方法获取为`Null`的现象.
