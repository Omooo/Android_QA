# Android_QA

### Java部分

1. ##### Java中 == 和 equals 和 hashCode 的区别

   Java中的数据类型可分为两类，基本数据类型和引用类型。基本数据类型：byte、short、char、int、long、float、double、boolean。他们之间的比较用双等号（==），比较的是**值**。引用类型：类、接口、数组。当他们用双等号（==）进行比较的时候，比较的是他们在**内存中的存放地址**。对象是放在堆中的，栈中存放的是对象的引用（地址）。由此可见，**双等号是对栈中的值进行比较的**。如果要比较堆中对象是否相同，那么就要重写equals方法了。

   默认情况下（没有覆盖equals方法）下的equals方法都是调用Object类的equals方法，而Object的equals方法主要是用于判断**对象的内存地址引用是不是同一个地址**（是不是同一个对象）。下面是Object类中的equals方法：

   ```java
   public boolean equals(Object obj) {  
       return (this == obj);  
   } 
   ```

   定义的equals方法与==是等效的。

   **但是**，要是类中覆盖了equals方法，那么就要根据具体代码来确定equals方法的作用了。**覆盖后的一般都是通过对象的内容是否相等来判断对象是否相等。**下面是String类对equals方法进行了重写：

   ```java
   public boolean equals(Object anObject) {  
       if (this == anObject) {  
           return true;  
       }  
       if (anObject instanceof String) {  
           String anotherString = (String)anObject;  
           int n = count;  
           if (n == anotherString.count) {  
           char v1[] = value;  
           char v2[] = anotherString.value;  
           int i = offset;  
           int j = anotherString.offset;  
           while (n-- != 0) {  
               if (v1[i++] != v2[j++])  
               return false;  
           }  
           return true;  
           }  
       }  
       return false;  
   }  
   ```

   hashCode()方法返回的就是一个数值，从方法名上来看，其目的就是生成一个hash码，hash码的主要用途就是在**对对象进行散列的时候作为key输入**。

   参考自：[http://blog.csdn.net/hla199106/article/details/46907725](http://blog.csdn.net/hla199106/article/details/46907725)

2. ##### int、char、long 各占多少字节数

   | 类型     | 位数   | 字节数  |
   | ------ | ---- | ---- |
   | boolen | 8    | 1    |
   | int    | 32   | 4    |
   | float  | 32   | 4    |
   | double | 64   | 8    |
   | char   | 16   | 2    |
   | byte   | 8    | 1    |
   | short  | 16   | 2    |
   | long   | 64   | 8    |

3. ##### int 和 Integer 的区别

   Java 为每一个基本数据类型都引入了对应的包装类，从Java 5 开始引入了自动装箱 / 拆箱机制，使得两者可以相互转化。**所以最基本的一点区别就是**：Integer 是int的包装类，int的初始值为零，而Integer的初值为null。int与Integer相比，会把Integer自动拆箱为int再去比。

   参考自：[http://blog.csdn.net/login_sonata/article/details/71001851](http://blog.csdn.net/login_sonata/article/details/71001851)

4. ##### 谈谈对Java多态的理解

   多态即：事物在运行过程中存在不同的状态。多态的存在有三个前提：要有继承关系、子类重写父类方法、父类数据类型的引用指向子类对象。弊端就是：不能使用子类特有的成员属性和子类特有的成员方法。如果非要用到不可，可以强制类型转换。

   参考自：[https://item.btime.com/m_2s22uri6z16](https://item.btime.com/m_2s22uri6z16)

5. ##### String、StringBuffer、StringBuilder的区别

   String：字符串常量，使用字符串拼接时是不同的两个空间。

   StringBuffer：字符串变量，线程安全，字符串拼接直接在字符串后追加。

   StringBuilder：字符串变量，非线程安全，字符串拼接直接在字符串后追加。

   StringBuilder执行效率高于StringBuffer高于String。String是一个常量，是不可变的，所以对于每次+=赋值都会创建一个新的对象，StringBuilder和StringBuffer都是可变的，当进行字符串拼接的时候采用append方法，在原来的基础上追加，所以性能要比String高，因为StringBuffer是线程安全的而StringBuilder是线程非安全的，所以StringBuilder的效率高于StringBuffer。对于大数据量的字符串拼接，采用StringBuffer、StringBuilder。另一种说法，JDK1.6做了优化，通过String声明的字符串在进行用+进行拼接时，底层调用的是StringBuffer，所以性能基本上和后两者没什么区别。

6. ##### 什么是内部类？内部类的作用

   Java 常见的内部类有四种：成员内部类、静态内部类、方法内部类和匿名内部类。

   内部类可以很好的实现隐蔽，一般的非内部类，是不允许有private和protectd等权限的，但内部类（除方法内部类）可以通过这些修饰符来实现隐蔽。

   内部类拥有外部类的访问权限（分静态与非静态情况），通过这一特性可以比较好的处理类之间的关联性，将一类事物的流程放在一起内部处理。

   通过内部类可以实现多重继承，Java默认是单继承的，我们可以通过多个内部类继承实现多个父类，接着由于外部类完全可访问内部类，所以就实现了类似多继承的效果。

   参考自：[https://www.jianshu.com/p/7fe3af7f0f2d](https://www.jianshu.com/p/7fe3af7f0f2d)

7. ##### 抽象类和接口的区别

   抽象类使用abstract class 定义，抽象类既可以有抽象方法也可以有其他类型的方法，既可以有静态属性也可以有非静态属性。

   接口使用interface定义，属性用public final static 修饰，如果没有写关键字，虚拟机默认会加上关键字。JDK8之后接口里的方法既可以有抽象方法也可以有默认方法。

   抽象类是一种类别，具有构造方法。接口是一种行为规范，没有构造方法。抽象类中可以没有抽象方法，他有自己的构造方法但是不能直接实例化对象，所以必须被子类继承之后才有意义。

   抽象类只能单继承，接口可以被多个类实现，接口和接口之间可以多继承。

   抽象类可以由默认的方法实现，接口根本不存在方法的实现。实现抽象类使用extends关键字来继承抽象类，如果子类不是抽象类，它需要提供抽象类中所有抽象方法的实现。子类使用关键字implements来实现接口，它需要提供接口中所有声明的方法的实现。

   抽象方法可以有public、protectd 和 default 这些修饰符，而接口方法默认修饰符是public，不可以使用其他修饰符。

   抽象类比接口速度快，接口是稍微有点慢的，这是因为它需要时间去寻找类中实现的方法。

   参考自：[http://www.importnew.com/12399.html](http://www.importnew.com/12399.html)

8. ##### 抽象类的意义

   为子类提供一个公共的类型；封装子类中重复的内容；定义有抽象方法，子类虽然有不同的实现，但该方法的定义是一致的；

9. ##### 抽象类与接口的应用场景

   如果你拥有一些方法并且想让它们中的一些默认实现，那么就用抽象类；如果你想实现多继承，那么必须实用接口；如果基本功能在不断变化，那么就需要使用抽象类，如果不断改变基本功能并且使用接口，那么就要改变所有实现了该接口的类。

10. ##### 抽象类是否可以没有方法和属性？

   可以，抽象类可以没有方法和属性，但是含有抽象方法的类一定是抽象类。

11. ##### 接口的意义

   对一些类是否具有某个功能非常关心，但不关心功能的具体实现，那么就需要这些类实现一个接口。

12. ##### 泛型中的extends和super的区别

   <? extends T>和<? super T>是泛型中的“通配符”和“边界”的概念。

​       <? extends T>：是指“上界通配符”，不能往里存，只能往外取。

​       <? super T>：是指“下界通配符”，不影响往里存，但往外取只能放在Object对象里。

​       **PECS原则：频繁往外读取内容，适合用上界Extends，经常往里插入的，适合用下界Super。**

​       参考自：[[Java]泛型中 extends 和 super 的区别？](https://itimetraveler.github.io/2016/12/27/%E3%80%90Java%E3%80%91%E6%B3%9B%E5%9E%8B%E4%B8%AD%20extends%20%E5%92%8C%20super%20%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F/)

13. ##### 父类的静态方法能否被子类重写？

    不能，子类继承父类后，非静态方法覆盖父类的方法，父类的静态方法被隐藏。

14. ##### 进程和线程的区别

​       进程是资源分配的基本单位，线程是处理器调度的基本单位。

​       进程拥有独立的地址空间，线程没有独立的地址空间，同一个进程内多个线程共享其资源。

​       线程划分尺度更小，所以多线程程序并发性更高。

​       一个程序至少有一个进程，一个进程至少有一个线程。

​       线程是属于进程的，当进程退出时该进程所产生的线程都会被强制退出并清除。

​       线程占用的资源要少于进程所占用的资源。

15.  ##### final、finally、finalize的区别

​       final用于声明属性、方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。

​       finally是异常处理语句结构的一部分，表示总是执行。

​       finalize是Object类的一个方法，在垃圾收集器执行的时候会回调被回收对象的finalize()方法，可以覆盖此方法提供垃圾收集时其他资源的回收，例如关闭文件。

16.  ##### 序列化的方式

​       实现Serializable接口和实现Parcelable接口。

17.  ##### Serializable 和 Parcelable 的区别

​       Serializable 是 Java的序列化接口。特点是简单，直接实现该接口就行了，其他工作都被系统完成了，但是对于内存开销大，序列化和反序列化需要很多的 I/O 流操作。

​       Parcelable 是Android的序列化方式，主要用于在内存序列化上，采用该方法需要实现该接口并且手动实现序列化过程，相对复杂点。如果序列化到存储设备（文件等）或者网络传输，比较复杂，建议用Serializable 方式。

​       **最大的区别就是存储媒介的不同：**Serializable 使用IO读写存储在硬盘上，Parcelable 是直接在内存中读写，内存读写速度远大于IO读写，所以Parcelable 更加高效。Serializable

​       在序列化过程中会产生大量的临时变量，会引起频繁的GC，效率低下。

18.  ##### 静态属性和静态方法是否可以被继承？是否可以被重写？以及原因。

​       静态属性和静态方法可以被继承，但是没有被重写而是被隐藏。

​       静态方法和属性是属于类的，调用的时候直接通过 类名.方法名 完成，不需要继承机制就可以调用。如果子类里面定义了静态方法和属性，那么这时候父类的静态方法或属性称之为“隐藏”。至于是否继承一说，子类是有继承静态方法和属性，但是跟实例方法和属性不太一样，存在隐藏这种情况。

​       多态之所以能够实现依赖于继承、接口和重写、重载（继承和重写最为关键）。**有了继承和重写就可以实现父类的引用指向不同子类的对象。** 重写的功能是：重写后子类的优先级高于父类的优先级，但是隐藏是没有这个优先级之分的。

​       静态属性、静态方法和非静态的属性都可以被继承和隐藏而不能被重写，因此不能实现多态，不能实现父类的引用可以指向不同子类的对象。

​       非静态方法可以被继承和重写，因此可以实现多态。

19.  ##### 静态内部类的设计意图

    ##### 

   ​