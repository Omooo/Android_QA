# Android_QA

## Java 部分（一）基础知识点

### 1. Java中 == 和 equals 和 hashCode 的区别

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



### 2. int、char、long 各占多少字节数

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



### 3. int 和 Integer 的区别

Java 为每一个基本数据类型都引入了对应的包装类，从Java 5 开始引入了自动装箱 / 拆箱机制，使得两者可以相互转化。**所以最基本的一点区别就是**：Integer 是int的包装类，int的初始值为零，而Integer的初值为null。int与Integer相比，会把Integer自动拆箱为int再去比。

参考自：[http://blog.csdn.net/login_sonata/article/details/71001851](http://blog.csdn.net/login_sonata/article/details/71001851)



### 4. 谈谈对Java多态的理解

多态即：事物在运行过程中存在不同的状态。多态的存在有三个前提：要有继承关系、子类重写父类方法、父类数据类型的引用指向子类对象。弊端就是：不能使用子类特有的成员属性和子类特有的成员方法。如果非要用到不可，可以强制类型转换。

参考自：[https://item.btime.com/m_2s22uri6z16](https://item.btime.com/m_2s22uri6z16)



### 5. String、StringBuffer、StringBuilder的区别

String：字符串常量，使用字符串拼接时是不同的两个空间。

StringBuffer：字符串变量，线程安全，字符串拼接直接在字符串后追加。

StringBuilder：字符串变量，非线程安全，字符串拼接直接在字符串后追加。

StringBuilder执行效率高于StringBuffer高于String。String是一个常量，是不可变的，所以对于每次+=赋值都会创建一个新的对象，StringBuilder和StringBuffer都是可变的，当进行字符串拼接的时候采用append方法，在原来的基础上追加，所以性能要比String高，因为StringBuffer是线程安全的而StringBuilder是线程非安全的，所以StringBuilder的效率高于StringBuffer。对于大数据量的字符串拼接，采用StringBuffer、StringBuilder。另一种说法，JDK1.6做了优化，通过String声明的字符串在进行用+进行拼接时，底层调用的是StringBuffer，所以性能基本上和后两者没什么区别。



### 6. 什么是内部类？内部类的作用

Java 常见的内部类有四种：成员内部类、静态内部类、方法内部类和匿名内部类。

内部类可以很好的实现隐蔽，一般的非内部类，是不允许有private和protectd等权限的，但内部类（除方法内部类）可以通过这些修饰符来实现隐蔽。

内部类拥有外部类的访问权限（分静态与非静态情况），通过这一特性可以比较好的处理类之间的关联性，将一类事物的流程放在一起内部处理。

通过内部类可以实现多重继承，Java默认是单继承的，我们可以通过多个内部类继承实现多个父类，接着由于外部类完全可访问内部类，所以就实现了类似多继承的效果。

参考自：[https://www.jianshu.com/p/7fe3af7f0f2d](https://www.jianshu.com/p/7fe3af7f0f2d)



### 7. 抽象类和接口的区别

抽象类使用abstract class 定义，抽象类既可以有抽象方法也可以有其他类型的方法，既可以有静态属性也可以有非静态属性。

接口使用interface定义，属性用public final static 修饰，如果没有写关键字，虚拟机默认会加上关键字。JDK8之后接口里的方法既可以有抽象方法也可以有默认方法。

抽象类是一种类别，具有构造方法。接口是一种行为规范，没有构造方法。抽象类中可以没有抽象方法，他有自己的构造方法但是不能直接实例化对象，所以必须被子类继承之后才有意义。

抽象类只能单继承，接口可以被多个类实现，接口和接口之间可以多继承。

抽象类可以由默认的方法实现，接口根本不存在方法的实现。实现抽象类使用extends关键字来继承抽象类，如果子类不是抽象类，它需要提供抽象类中所有抽象方法的实现。子类使用关键字implements来实现接口，它需要提供接口中所有声明的方法的实现。

抽象方法可以有public、protectd 和 default 这些修饰符，而接口方法默认修饰符是public，不可以使用其他修饰符。

抽象类比接口速度快，接口是稍微有点慢的，这是因为它需要时间去寻找类中实现的方法。

参考自：[http://www.importnew.com/12399.html](http://www.importnew.com/12399.html)



### 8. 抽象类的意义

为子类提供一个公共的类型；封装子类中重复的内容；定义有抽象方法，子类虽然有不同的实现，但该方法的定义是一致的；



### 9. 抽象类与接口的应用场景

如果你拥有一些方法并且想让它们中的一些默认实现，那么就用抽象类；如果你想实现多继承，那么必须实用接口；如果基本功能在不断变化，那么就需要使用抽象类，如果不断改变基本功能并且使用接口，那么就要改变所有实现了该接口的类。



### 10. 抽象类是否可以没有方法和属性？

可以，抽象类可以没有方法和属性，但是含有抽象方法的类一定是抽象类。



### 11. 接口的意义

对一些类是否具有某个功能非常关心，但不关心功能的具体实现，那么就需要这些类实现一个接口。



### 12. 泛型中的extends和super的区别

```
<? extends T>和<? super T>是泛型中的“通配符”和“边界”的概念。
<? extends T>：是指“上界通配符”，不能往里存，只能往外取。
<? super T>：是指“下界通配符”，不影响往里存，但往外取只能放在Object对象里。
```

**PECS原则：频繁往外读取内容，适合用上界Extends，经常往里插入的，适合用下界Super。**

参考自：[[Java]泛型中 extends 和 super 的区别？](https://itimetraveler.github.io/2016/12/27/%E3%80%90Java%E3%80%91%E6%B3%9B%E5%9E%8B%E4%B8%AD%20extends%20%E5%92%8C%20super%20%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F/)



### 13. 父类的静态方法能否被子类重写？

不能，子类继承父类后，非静态方法覆盖父类的方法，父类的静态方法被隐藏。



### 14. 进程和线程的区别

进程是资源分配的基本单位，线程是处理器调度的基本单位。

进程拥有独立的地址空间，线程没有独立的地址空间，同一个进程内多个线程共享其资源。

线程划分尺度更小，所以多线程程序并发性更高。

一个程序至少有一个进程，一个进程至少有一个线程。

线程是属于进程的，当进程退出时该进程所产生的线程都会被强制退出并清除。

线程占用的资源要少于进程所占用的资源。



### 15. final、finally、finalize的区别

final用于声明属性、方法和类，分别表示属性不可变，方法不可覆盖，类不可继承。

finally是异常处理语句结构的一部分，表示总是执行。

finalize是Object类的一个方法，在垃圾收集器执行的时候会回调被回收对象的finalize()方法，可以覆盖此方法提供垃圾收集时其他资源的回收，例如关闭文件。



### 16. 序列化的方式

实现Serializable接口和实现Parcelable接口。



### 17. Serializable 和 Parcelable 的区别

Serializable 是 Java的序列化接口。特点是简单，直接实现该接口就行了，其他工作都被系统完成了，但是对于内存开销大，序列化和反序列化需要很多的 I/O 流操作。

Parcelable 是Android的序列化方式，主要用于在内存序列化上，采用该方法需要实现该接口并且手动实现序列化过程，相对复杂点。如果序列化到存储设备（文件等）或者网络传输，比较复杂，建议用Serializable 方式。

**最大的区别就是存储媒介的不同：**Serializable 使用IO读写存储在硬盘上，Parcelable 是直接在内存中读写，内存读写速度远大于IO读写，所以Parcelable 更加高效。Serializable在序列化过程中会产生大量的临时变量，会引起频繁的GC，效率低下。



### 18. 静态属性和静态方法是否可以被继承？是否可以被重写？以及原因。

静态属性和静态方法可以被继承，但是没有被重写而是被隐藏。

静态方法和属性是属于类的，调用的时候直接通过 类名.方法名 完成，不需要继承机制就可以调用。如果子类里面定义了静态方法和属性，那么这时候父类的静态方法或属性称之为“隐藏”。至于是否继承一说，子类是有继承静态方法和属性，但是跟实例方法和属性不太一样，存在隐藏这种情况。

多态之所以能够实现依赖于继承、接口和重写、重载（继承和重写最为关键）。**有了继承和重写就可以实现父类的引用指向不同子类的对象。** 重写的功能是：重写后子类的优先级高于父类的优先级，但是隐藏是没有这个优先级之分的。

静态属性、静态方法和非静态的属性都可以被继承和隐藏而不能被重写，因此不能实现多态，不能实现父类的引用可以指向不同子类的对象。

非静态方法可以被继承和重写，因此可以实现多态。



### 19. 静态内部类的设计意图

只是为了降低包的深度，方便类的使用，静态内部类适用于包含类当中，但又不依赖于外在的类，不用使用外在类的非静态属性和方法，只是为了方便管理类结构而定义。在创建静态内部类的时候，不需要外部类对象的引用。

非静态内部类有一个很大的优点：可以自由使用外部类中的变量和方法。

```java
static class Outer {
	class Inner {}
	static class StaticInner {}
}

Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();
Outer.StaticInner inner0 = new Outer.StaticInner();
```

参考自：[https://www.zhihu.com/question/28197253](https://www.zhihu.com/question/28197253)



### 20. 成员内部类、静态内部类、方法内部类（局部内部类）和匿名内部类的理解，以及项目中的应用

成员内部类：

​	最普通的内部类，它的定义位于一个类的内部，这样看起来，成员内部类相当于外部类的一个成员，成员内部类可以无条件访问外部类的所有成员属性和成员方法（包括private成员和静态成员）。不过需要注意的是，当成员内部类拥有和外部类同名的成员变量或者方法时，会发生隐藏现象，即默认情况下访问的是成员内部类的成员。如果要访问外部类的同名成员，形式如下：

```java
外部类.this.成员变量
外部类.this.成员方法
```

虽然成员内部类可以无条件的访问外部类的成员，而外部类想访问成员内部类的成员却不是那么随心所欲了。成员内部类是依附外部类而存在的，也就是说，如果要创建成员内部类的对象，前提必须存在一个外部类对象。创建成员内部类对象的一般方式如下：

```java
public class Test {
    public static void main(String[] args)  {
        //第一种方式：
        Outter outter = new Outter();
        Outter.Inner inner = outter.new Inner();  //必须通过Outter对象来创建
         
        //第二种方式：
        Outter.Inner inner1 = outter.getInnerInstance();
    }
}
 
class Outter {
    private Inner inner = null;
    public Outter() {
         
    }
     
    public Inner getInnerInstance() {
        if(inner == null)
            inner = new Inner();
        return inner;
    }
      
    class Inner {
        public Inner() {
             
        }
    }
}
```

内部类可以拥有private、protected、public以及包访问权限。如果成员内部类用private修饰，则只能在外部类的内部访问，如果用public修饰，则任何地方都能访问，如果用protected修饰，则只能在同一个包下或者继承外部类的情况下访问，如果是默认访问权限，则只能是同一个包下访问。这一点和外部类有一点不一样，外部类只能被public和包访问两种权限修饰。由于成员内部类看起来像外部类的一个成员，所以可以像类的成员一样拥有多种修饰权限。

静态内部类：

​	静态内部类也是定义在另一个类里面的类，只不过在类的前面多了一个关键字static。静态内部类是不需要依赖外部类的，这点和类的静态成员属性有点相似，并且它不能使用外部类的非static成员变量或者方法。这点很好理解，因为在没有外部类的对象的情况下，可以创建静态内部类的对象，如果允许访问外部类的非static成员就会产生矛盾，因为外部类的非static成员必须依赖于具体的对象。

局部内部类：

​	局部内部类是定义在一个方法或者一个作用域里面的的类，它和成员内部类的区别在于局部内部类的访问权限仅限于方法内或者该作用域内。注意，局部内部类就像是方法里面的一个局部变量一样，是不能有public、protected、private以及static修饰符的。

匿名内部类：

​	匿名内部类不能有访问修饰符和static修饰符的，一般用于编写监听代码。匿名内部类是唯一一个没有构造器的类。正因为没有构造器，所以匿名内部类的适用范围非常有限，大部分匿名内部类用于接口回调。一般来说，匿名内部类用于继承其他类或者实现接口，并不需要增加额外的方法，只是对继承方法的实现或者是重写。

应用场景：

1. 最重要的一点，每个内部类都能独立的继承一个接口的实现，无论外部类是否已经继承了某个接口的实现，对于内部类都没有影响。内部类使得多继承的解决方案变得完整。

 	2. 方便将存在一定逻辑关系的类组织在一起，又可以对外界隐蔽。
 	3. 方便编写事件驱动程序
 	4. 方便编写线程代码

总结：

​	对于成员内部类，必须先产生外部类的实例化对象，才能产生内部类的实例化对象。而非静态内部类不用产生外部类的实例化对象即可产生内部类的实例化对象。

```java
创建静态内部类对象的一般形式：
外部类类名.内部类类名 xxx = new 外部类类名.内部类类名()
创建成员内部类对象的一般形式：
外部类类名.内部类类名 xxx = new 外部类对象名.new 内部类类名()
```

参考自：[http://www.cnblogs.com/latter/p/5665015.html](http://www.cnblogs.com/latter/p/5665015.html)



### 21. 谈谈对kotlin的理解

挖个坑，以后自己填！哼哼（傲娇脸



### 22. 闭包和局部内部类的区别



### 23. String转换成Integer的方式以及原理

String转int：

```java
int i = Integer.parseInt(s);
int i = Integer.valueOf(s).intValue();
```

int转String：

```java
String s = i + "";
String s = Integer.toString(i);
String s = String.valueOf(i);
```

源码：

```java
public static int parseInt(String s, int radix)  throws NumberFormatException  
    {  
        /* 
         * WARNING: This method may be invoked early during VM initialization 
         * before IntegerCache is initialized. Care must be taken to not use 
         * the valueOf method. 
         */  
    // 下面三个判断好理解，其中表示进制的 radix 要在（2~36）范围内  
        if (s == null) {  
            throw new NumberFormatException("null");  
        }  
  
        if (radix < Character.MIN_RADIX) {  
            throw new NumberFormatException("radix " + radix +  
                                            " less than Character.MIN_RADIX");  
        }  
  
        if (radix > Character.MAX_RADIX) {  
            throw new NumberFormatException("radix " + radix +  
                                            " greater than Character.MAX_RADIX");  
        }  
  
        int result = 0; // 表示结果， 在下面的计算中会一直是个负数，   
            // 假如说 我们的字符串是一个正数  "7" ，   
            // 那么在返回这个值之前result保存的是 -7，  
            // 这个可能是为了保持正数和负数在下面计算的一致性  
        boolean negative = false;  
        int i = 0, len = s.length();  
  
  
    //limit 默认初始化为 最大正整数的  负数 ，假如字符串表示的是正数，  
    //那么result(在返回之前一直是负数形式)就必须和这个最大正数的负数来比较，判断是否溢出  
  
        int limit = -Integer.MAX_VALUE;   
              
        int multmin;  
        int digit;  
  
        if (len > 0) {     // 首先是对第一个位置判断，是否含有正负号  
            char firstChar = s.charAt(0);  
            if (firstChar < '0') { // Possible leading "+" or "-"  
                if (firstChar == '-') {  
                    negative = true;  
              
            // 这里，在负号的情况下，判断溢出的值就变成了 整数的 最小负数了。  
                    limit = Integer.MIN_VALUE;  
  
                } else if (firstChar != '+')  
                    throw NumberFormatException.forInputString(s);  
  
                if (len == 1) // Cannot have lone "+" or "-"  
                    throw NumberFormatException.forInputString(s);  
                i++;  
            }  
  
            multmin = limit / radix;  
        // 这个是用来判断当前的 result 在接受下一个字符串位置的数字后会不会溢出。  
        //  原理很简单，为了方便，拿正数来说  
        //  （multmin result 在计算中都是负数），假如是10  
        // 进制,假设最大的10进制数是 21，那么multmin = 21/10 = 2,   
        //  如果我此时的 result 是 3 ，下一个字符c来了，result即将变成  
        // result = result * 10 + c;那么这个值是肯定大于 21 ，即溢出了，  
            //  这个溢出的值在 int里面是保存不了的，不可能先计算出  
        // result（此时的result已经不是溢出的那个值了） 后再去与最大值比较。    
        // 所以要通过先比较  result < multmin   （说明result * radix 后还比 limit 小）     
  
            while (i < len) {  
                // Accumulating negatively avoids surprises near MAX_VALUE  
                digit = Character.digit(s.charAt(i++),radix);  
                if (digit < 0) {  
                    throw NumberFormatException.forInputString(s);  
                }  
  
  
        // 这里就是上说的判断溢出，由于result统一用负值来计算，所以用了 小于 号  
        // 从正数的角度看就是   reslut > mulmin  下一步reslut * radix 肯定是 溢出了  
                if (result < multmin) {   
                    throw NumberFormatException.forInputString(s);  
                }  
  
  
  
                result *= radix;  
  
        // 这里也是判断溢出， 由于是负值来判断，相当于 （-result + digit）> - limit  
        // 但是不能用这种形式，如果这样去比较，那么得出的值是肯定判断不出溢出的。  
        // 所以用    result < limit + digit    很巧妙  
                if (result < limit + digit) {  
  
                    throw NumberFormatException.forInputString(s);  
                }  
                result -= digit; // 加上这个 digit 的值 （这里减就相当于加）  
            }  
        } else {  
            throw NumberFormatException.forInputString(s);  
        }  
        return negative ? result : -result; // 正数就返回  -result   
    } 
```

参考自：[http://blog.csdn.net/stu_wanghui/article/details/38564177](http://blog.csdn.net/stu_wanghui/article/details/38564177)



## Java 部分（二）高级知识点

### 1. 哪些情况下的对象会被垃圾回收机制处理掉？

