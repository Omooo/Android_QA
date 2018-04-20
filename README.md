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

规范、扩展、回调。

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

垃圾回收机制中最基本的做法是分代回收。内存区域被划分为三代：新生代、老年代和永久代。对于不同世代可以使用不同的垃圾回收算法。一般来说，一个应用中的大部分对象的存活时间都很短，基于这一点，对于新生代的垃圾回收算法就可以很有针对性。

参考自：[Java — 垃圾收集灵魂拷问三连（一）](http://omooo.top/2017/12/25/Java%20---%20%E5%85%B3%E4%BA%8E%E5%9E%83%E5%9C%BE%E6%94%B6%E9%9B%86%E7%81%B5%E9%AD%82%E6%8B%B7%E9%97%AE%E4%B8%89%E8%BF%9E%EF%BC%88%E4%B8%80%EF%BC%89)

### 2. 讲一下常见的编码方式？

编码的原因：

​	计算机存储信息的最小单元是一个字节即八个bit，所以能表示的字符范围是0 - 225 个。然而要表示的符号太多了，无法用一个字节来完全表示，要解决这个矛盾必须需要一个新的数据结构char，从char到byte必须编码。编码方式规定了转化的规则，按照这个规则就可以让计算机正确的表示我们的字符。编码方式有以下几种：

ASCII码：

​	总共有128个，用一个字节的低七位表示，0 - 31是控制字符，如回车键、换行等等。32 - 126是打印字符，可以通过键盘输入并且能够显示出来。

ISO-8859-1：

​	ISO组织在ASCII码基础上又制定了一些列标准用来扩展ASCII编码，其中IS-8859-1涵盖了大多数西欧语言字符，单字节编码，总共表示256个字节，应用广泛。

GB2312、GBK：

​	双字节编码，表示汉字。

UTF-16：

​	UTF - 16具体定义了Unicode字符在计算机中的存取方法。UTF - 16用两个字节来表示Unicode转化格式，这个是定长的表示方法，不论什么字符都可以用两个字节表示，两个字节是十六个bit，所以叫UTF - 16.

UTF- 8：

UTF-16统一采用两个字节表示一个字符，虽然表示上非常简单方便，但是有很大一部分字符用一个字节就可以表示，显然是浪费了存储空间。于是UTF-8应运而生，UTF-8采用一种变长技术，每个编码区域有不同的字码长度，不同类型的字符可以是由1-6个字节组成。

参考自：[几种常见的编码格式](https://www.cnblogs.com/mlan/p/7823375.html)



### 3. UTF-8编码中中文占几个字节，int型几个字节？

中文占字节数可以是2、3和4个的，最高为4个字节。

参考自：[http://blog.csdn.net/hellokatewj/article/details/24325653](http://blog.csdn.net/hellokatewj/article/details/24325653)



### 4. 静态代理和动态代理的区别，什么场景使用？

参考自：[https://www.jianshu.com/p/2f518a4a4c2b](https://www.jianshu.com/p/2f518a4a4c2b)



### 5. Java的异常体系

异常简介：异常是阻止当前方法继续执行的问题，如：文件找不到、网络连接失败、非法参数等等。发现异常的理想时期是编译阶段，然而编译阶段不能找出所有的异常，余下的问题必须在运行期间。

Java中的异常分为可查异常和不可查异常。

**可查异常：**

​	即编译时异常，只编译器在编译时可以发现的错误，程序在运行时很容易出现的异常状况，这些异常可以预计，所以在编译阶段就必须手动进行捕捉处理，即要用try-catch语句捕获它，要么用throws子句声明抛出，否者编译无法通过，如：IOException、SQLException以及用户自定义Exception异常。

**不可查异常：**

​	不可查异常包括**运行时异常**和**错误**，他们都是在程序运行时出现的。异常和错误的区别：异常能被程序本身处理，错误是无法处理的。**运行时异常指的是**程序在运行时才会出现的错误，由程序员自己分析代码决定是否用try...catch进行捕获处理。如空指针异常、类转换异常、数组下标越界异常等等。**错误是指**程序无法处理的错误，表示运行应用程序中较严重的问题，如系统崩溃、虚拟机错误、动态连接失败等等。大多数错误与代码编写者执行的操作无关，而表示代码运行时JVM出现的问题。

**Java异常类层次结构图：**

![](https://upload-images.jianshu.io/upload_images/81578-ce7c03b8b1930315.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/600)

从上图可以看出Java通过API中Throwable类的众多子类描述各种不同的异常。因而，Java异常都是对象，是Throwable子类的实例。

**异常处理机制：**

当异常发生时，将使用new在堆上创建一个异常对象，对于这个异常对象，有两种处理方式：

1. 使用throw关键字将异常对象抛出，则当前执行路径被终止，异常处理机制将在其他地方寻找catch快对异常进行处理
2. 使用try...catch在当前逻辑中进行捕获处理

**throws 和 throw：**

throws：一个方法在声明时可以使用throws关键字声明可能会产生的若干异常

throw：抛出异常，并退出当前方法或作用域

使用 finally 清理：

在 Java 中的 finally 的存在并不是为了释放内存资源，因为 Java 有垃圾回收机制，因此需要 Java 释放的资源主要是：已经打开的文件或者网络连接等。

在 try 中无论有没有捕获异常，finally 都会被执行。

参考自：[https://www.jianshu.com/p/ffca876ce719](https://www.jianshu.com/p/ffca876ce719)



### 6. 谈谈你对解析与分派的认识

[深入理解Java虚拟机之：多态性实现机制----静态分派与动态分配](https://www.jianshu.com/p/1976b01c07d2)



### 7. 修改对象A的equals方法的签名，那么使用HashMap存放这个对象实例的时候，会调用哪个equals方法？



### 8. Java中实现多态的机制是什么？



### 9. 如何将一个Java对象序列化到文件里？



### 10. 说说你对Java反射的理解

反射：在运行状态中，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意一个对象，都能够调用它的任意一个方法和属性；这种动态获取的信息以及动态调用对象的方法的功能称为 Java 语言的反射机制。

关于反射的基本使用可看：[Java-反射的理解与使用-(原创)](https://www.jianshu.com/p/2302ad5dbfad)

**反射是一种具有与类进行动态交互能力的一种机制**。

**反射的作用：**

在 Java 和 Android 开发中，一般情况下下面几种场景会用到反射机制：

* 需要访问隐藏属性或者调用方法改变程序原来的逻辑，这个在开发中是很常见的，由于一些原因，系统并没有开放一些接口出来，这个时候利用反射是一个有效的解决办法。
* 自定义注解，注解就是在运行时利用反射机制来获取的。
* 在开发中动态加载类，比如在 Android 中的动态加载解决65k问题等等，模块化和插件化都离不开反射。

**反射的工作原理**

我们知道，每个 Java 文件最终都会被编译成一个 .class 文件，这些 Class 对象承载了这个类的所有信息，包括父类、接口、构造函数、方法、属性等等，这些class文件在程序运行时会被 ClassLoader 加载到虚拟机中。当一个类被加载以后，Java 虚拟机就会在内存中自动产生一个 Class 对象，而一般情况下用 new 来创建对象，实际上本质都是一样的，只是底层原理对我们开发者透明罢了。有了 class 对象的引用，就相当于有了 Method、Field、Constructor 的一切信息，在 Java 中，有了对象的引用就有了一切，剩下就靠自己发挥了。

参考自：[Java反射以及在Android中的特殊应用](https://www.jianshu.com/p/8f394d90a85c)



### 11. 说说你对Java注解的理解

注解（Annotion）是一个接口（前加 @），程序可以通过反射来获取指定程序元素的Annotion对象，然后通过Annotion对象来获取注解里面的元数据。

注解可以用于创建文档，跟踪代码中的依赖性，甚至执行基本编译时检查。从某些方面看，Annotion就像修饰符一样被使用，并应用于包、类型、构造方法、方法、成员变量、参数、本地变量的声明中。

Annotation 的行为十分类似 public、final 这样的修饰符。每个 Annotation 具有一个名字和成员，每个 Annotation 的成员具有被称为 name=value 对的名字和值，name=value 装载了 Annotation 的信息，也就是说注解中可以不存在成员。

使用注解的基本规则：Annotation 不能影响程序代码的执行，无论增加、删除 Annotation，代码都始终如一的执行。

关于注解的基本使用可见：[https://www.jianshu.com/p/91393daaaf32](https://www.jianshu.com/p/91393daaaf32)

参考自：[https://www.jianshu.com/p/8da24b7cf443](https://www.jianshu.com/p/8da24b7cf443)



### 12. 说说你对依赖注入的理解

依赖注入：可以通过这个服务来安全的注入组件到应用程序中，在应用程序部署的时候还可以选择从特定的接口属性进行注入。

参考自：

[用Dagger2在Android中实现依赖注入](https://www.jianshu.com/p/f713dd40e037)

[反射、注解与依赖注入总结](https://www.jianshu.com/p/24820bf3df5c)



### 13. 说一下泛型原理，并举例说明



### 14. Java中String的了解



### 15. String为什么要设计成不可变的？

String 是 Java 中一个不可变类，所以他一旦被实例化就无法被修改。为什么要把String设计成不可变的呢？那就从内存、同步和数据结构层面上谈起。

**字符串池**

字符串池是方法区中的一部分特殊存储。当一个字符串被创建的时候，首先会去这个字符串池中查找。如果找到，直接返回对该字符串的引用。

下面的代码只会在堆中创建一个字符串：

```java
String string1 = "abcd";
String string2 = "abcd";
```

![](http://www.hollischuang.com/wp-content/uploads/2016/03/QQ20160302-3.png)

**如果字符串可变的话，当两个引用指向同一个字符串时，对其中一个做修改就会影响另外一个。**

```java
String str= "123";
str = "456";
```

执行过程如下：

![](https://upload-images.jianshu.io/upload_images/2466095-96d3d70753c70c1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

执行第一行代码时，在堆上新建一个对象实例 123，str 是一个指向该实例的引用，**引用包含的仅仅只是实例在堆上的内存地址而已**。执行第二行代码时，仅仅只是改变了 str 这个引用的地址，指向了另外一个实例 456。**给 String 赋值仅仅只是改变了它的引用而已，并不会真正的去改变它本来内存地址上的值**。这样的好处也是显而易见的，最简单的当存在多个 String 的引用指向同一个内存地址时，改变其中一个引用的值并不会其他引用的值造成影响。

那如果我们这样写呢：

```java
  String str1 = "123";
  String str2 = new String("123");
  System.out.println(str1 == str2);
```

结果是 false。JVM为了字符串的复用，减少字符串对象的重复创建，特别维护了一个字符串常量池。第一种字面量形式的写法，会直接在字符串常量池中查找是否存在值 123，若存在直接返回这个值的引用，若不存在则创建一个值123的String对象并存入字符串常量池中。而使用new关键字，则会直接才堆上生成一个新的String对象，并不会理会常量池中是否有这个值。所以本质上 str1 和 str2 指向的内存地址是不一样的。

那么，使用 new 关键字生成的 String 对象可以进入字符串常量池吗？答案是肯定的，String 类提供了一个 native 方法 intern() 用来将这个对象加入字符串常量池：

```java
  String str1 = "123";
  String str2 = new String("123");
  str2=str2.intern();
  System.out.println(str1 == str2);
```

结果为 true。str2 调用 intern() 方法后，首先会在字符串常量池中寻找是否存在值为 123 的对象，若存在直接返回该对象的引用，若不存在，加入 str2 并返回。上诉代码中，常量池中已经存在了值为 123 的 str1 对象，则直接返回 str1 的引用地址，使得 str1 和 str2 指向同一个内存地址。

**缓存 Hashcode**

Java 中经常会用到字符串的哈希码。例如，在HashMap中，字符串的不可变能保证其Hashcode永远保持一致，避免了一些不必要的麻烦。这也就意味着每次使用一个字符串的hashcode的时候不用重新计算一次，更加高效。

**安全性**

String被广泛的使用在其他类中充当参数，如果字符串可变，那么类似操作就会可能导致安全问题。可变的字符串也可能导致反射的安全问题，因为它的参数也是字符串。

**不可变对象天生就是线程安全的**

因为不可变对象不能被改变，所以他们可以自由的在多个线程之间共享，不需要做任何同步处理。

总之，String被设计成不可变的主要目的是为了安全和高效。当然，缺点就是要创建多余的对象而并非改变其值。

参考自：

[https://www.jianshu.com/p/b1d62928552d](https://www.jianshu.com/p/b1d62928552d)

[http://www.hollischuang.com/archives/1246](http://www.hollischuang.com/archives/1246)



### 16. Object类的equal和hashCode方法重写，为什么？



## Java 部分（三）数据结构

### 1. 常用数据结构简介



### 2. 并发集合了解哪些？



## Java 部分（四）线程、多线程和线程池

### 1. 开启线程的三种方式？



### 2. 线程和进程的区别？



### 3. 为什么要有线程，而不是仅仅用进程？



### 4. run() 和 start() 方法的区别？



### 5. 如何控制某个方法允许并发访问线程的个数？



### 6. 在 Java 中 wait() 和 sleep() 方法的区别



### 7. 谈谈 wait/notify 关键字的理解



### 8. 什么导致线程阻塞？



### 9. 线程如何关闭？



### 10. 讲一下 Java 中的同步的方法



### 11. 数据一致性如何保证？



### 12. 如何保证线程安全？



### 13. 如何实现线程同步？



### 14. 两个进程同时要求读写，能不能实现？如何防止进程同步？



### 15. 线程间操作 List



### 16. Java 中对象的生命周期



### 17. Synchronized 的用法



### 18. Synchronized 的原理





## Android 部分（一）基础知识点

### 1. 四大组件是什么？

四大组件：Activity、Service、BroadcastReceiver、ContentProvider。

### 2. 四大组件的生命周期和简单用法

### **Activity：**

典型的生命周期好像没什么可说的，主要说一下特殊情况下的生命周期。

1. 横竖屏的切换

   在横竖屏切换的过程中，会发生Activity被销毁并被重建的过程。

   在了解这种情况下的生命周期，首先应该了解这两个回调：onSaveInstance State 和 onRestoreInstance State。

   在Activity由于异常情况下终止时，系统会调用 onSaveInstanceState 来保存当前 Activity 的状态。这个方法的调用是在onStop之前，它和onPause没有既定的时序关系，该方法只有在Activity被异常终止的情况下调用。当异常终止的Activity被重建之后，系统会调用onRestoreInstanceState，并且把Activity销毁时onSaveInstanceState方法所保存的Bundle对象参数同时传递给onRestoreInstanceState和onCreate方法。因为，可以通过onRestoreInstanceState方法来恢复Activity的状态，该方法的调用时机是在onStart之后。其中，onCreate和onRestoreInstanceState方法来恢复Activity状态的区别：onRestoreInstanceState回调则表明其中Bundle对象非空，不用加非空判断，而onCreate需要非空判断，建议使用onRestoreInstanceState。

![](http://upload-images.jianshu.io/upload_images/3985563-23d90471fa7f12d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​	横竖屏切换的生命周期：onPause() --> onSaveInstanceState() --> onStop() --> onDestory() --> onCreate() --> onStart() --> onRestoreInstanceState() --> onResume() 

​	可以通过在AndroidManifest文件的Activity中指定如下属性：

​	android:configChanges = "orientation| screenSize"

​	来避免横竖屏切换时，Activity的销毁和重建，而是回调了下面的方法：

```java
	@Override
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
    }
```

2. 资源内存不足导致优先级低的Activity被杀死

   优先级：前台Activity > 可见但非前台Activity > 后台Activity

**启动模式：**

Android 提供了四种Activity启动方式：

标准模式：standard

栈顶复用模式：singleTop

栈内复用模式：singleTask

单例模式：singleInstance

1. 标准模式 standard

   每启动一次Activity，就会创建一个新的Activity实例并置于栈顶。谁启动了这个Activity，那么这个Activity就运行在启动它的那个Activity所在的栈中。

   特殊情况下，如果在Service或Application中启动一个Activity，其并没有所谓的任务栈，可以使用标记位Flag来解决。解决办法：为待启动的Activity指定FLAG_ACTIVITY_NEW_TASK标记位，创建一个新栈。

2. 栈顶复用模式 singleTop

   如果需要新建的Activity位于任务栈栈顶，那么此Activity的实例就不会重建，而是复用栈顶的实例。并回调：

   ```java
       @Override
       protected void onNewIntent(Intent intent) {
           super.onNewIntent(intent);
       }
   ```

   由于不会重建一个Activity实例，则不会回调其他生命周期方法。

   应用场景：在通知栏点击收到的通知，然后需要启动一个Activity，这个Activity就可以用singleTop，否则每次点击都会新建一个Activity。

3. 栈内复用模式 singleTask

   该模式是一种单例模式，即一个栈内只有一个该Activity实例。该模式，可以通过在AndroidManifest文件的Activity中指定该Activity需要加载到哪个栈中，即singleTask的Activity可以指定想要加载的目标栈。singleTask和taskAffinity配合使用，指定开启的Activity加入到哪个栈中。

   ```xml
   <activity android:name=".Activity1"
       android:launchMode="singleTask"
       android:taskAffinity="com.lvr.task"
       android:label="@string/app_name">
   </activity>
   ```

   关于taskAffinity的值：每个Activity都有taskAffinify属性，这个属性指出了它希望进入的Task。如果一个Activity没有显式的指明该Activity的taskAffinity，那么它的这个属性就等于Application指明的taskAffinity，如果Application也没有指明，那么该taskAffinity的值就等于包名。

   执行逻辑：

   在这种模式下，如果Activity指定的栈不存在，则创建一个栈，并把创建的Activity压入栈内。如果Activity指定的栈存在，如果其中没有该Activity实例，则会创建Activity并压入栈顶，如果其中有该Activity实例，则把该Activity实例之上的Activity杀死清除出战，重用并让该Activity实例处在栈顶，然后调用onNewIntent()方法。

   应用场景：

   在大多数App的主页，对于大部分应用，当我们在主界面点击返回按钮都是退出应用，那么当我们第一次进入主界面之后，主界面位于栈底，以后不管我们打开了多少个Activity，只要我们再次回到主界面，都应该使用将主界面Activity上所有的Activity移除的方式来让主界面Activity处于栈顶，而不是往栈顶新加一个主界面Activity的实例，通过这种方式能够保证退出应用时所有的Activity都能被销毁。

4. 单例模式 singleInstance

   作为栈内复用的加强版，打开该Activity时，直接创建一个新的任务栈，并创建该Activity实例放入栈中。一旦该模式的Activity实例已经存在于某个栈中，任何应用在激活该Activity时都会重用该栈中的实例。

   应用场景：呼叫来电界面

**特殊情况：前台栈和后台栈的交互**

![](http://upload-images.jianshu.io/upload_images/3985563-4aeb1947bba27e44.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/3985563-f2eaf1005cdf1b1d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

调用SingleTask模式的后台任务栈的Activity，会把整个栈的Activity压入当前栈的栈顶。singleTask会具有clearTop特性，会把之上的栈内Activity清除。

**Activity的Flags：**

Activity的Flags很多，这里介绍集中常用的，用于设定Activity的启动模式，可以在启动Activity时，通过Intent.addFlags()方法设置。

1. FLAG_ACTIVITY_NEW_TASK 即 singleTask
2. FLAG_ACTIVITY_SINGLE_TOP 即 singleTop
3. FLAG_ACTIVITY_CLEAR_TOP 当他启动时，在同一个任务栈中所有位于它之上的Activity都要出栈。如果和singleTask模式一起出现，若被启动的Activity已经存在栈中，则清除其之上的Activity，并调用该Activity的onNewIntent方法。如果被启动的Activity采用standard模式，那么该Activity连同之上的所有Activity出栈，然后创建新的Activity实例并压入栈中。



### Service

是Android中实现程序后台运行的解决方案，它非常适用于去执行那些不需要和用户交互而且还要长期运行的任务。Service默认并不会运行在子线程中，它也不运行在一个独立的进程中，它同样执行在UI线程中，因此，不要在Service中执行耗时的操作，因此，不要在Service中执行耗时的操作，除非你在Service中创建了子线程来完成耗时操作。





