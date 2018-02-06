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
   | long   | 64   |      |

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

   ​