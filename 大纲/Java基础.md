## Java基础

### String buffer 与String builder 的区别

http://www.importnew.com/18167.html

http://www.importnew.com/15856.html

String 类型和StringBuffer的主要性能区别：String是不可变的对象, 因此在每次对String 类型进行改变的时候，都会生成一个新的 String 对象，然后将指针指向新的 String 对象，所以经常改变内容的字符串最好不要用 String ，因为每次生成对象都会对系统性能产生影响，特别当内存中无引用对象多了以后， JVM 的 GC 就会开始工作，性能就会降低。

StringBuffer 是线程安全的, StringBuilder 不是线程安全的。



### String 为什么要设计成不可变的

String类是final的,其内部主要是由char数组来实现,其中的相关操作,例如substring,concat,replace,都是重新生成了一个新的字符串对象(可参见源码),最原始的字符串并没有被改变

不可变的好处最简单的原因就是安全,String是几乎每个类都会使用的类,如果他被改变,会产生很多危险,例如hashMap中的key



### object类的equal 和hashcode 方法重写，为什么

http://www.importnew.com/25783.html

http://www.importnew.com/18851.html

equlas()和hashcode()都是可重写的方法

Object类中equals()方法是比较对象是否相等,hashcode()是返回了一个hash值,是一个native方法

所以只要不重写equals的话,两个对象则一定返回false

hashCode方法就是根据一定的规则将与对象相关的信息（比如对象的存储地址，对象的字段等）映射成一个数值，这个数值称作为散列值



所以当重写equlas方法时,有些想让两个不同的地址的对象相等,就必须要重写hashcode方法,因为hashcode值已经不一样了

```java
package com.cxh.test1;
 
import java.util.HashMap;
import java.util.HashSet;
import java.util.Set;
 
class People{
    private String name;
    private int age;
 
    public People(String name,int age) {
        this.name = name;
        this.age = age;
    }  
 
    public void setAge(int age){
        this.age = age;
    }
 
    @Override
    public boolean equals(Object obj) {
        // TODO Auto-generated method stub
        return this.name.equals(((People)obj).name) && this.age== ((People)obj).age;
    }
    
    //@Override
    //public int hashCode() {
        // TODO Auto-generated method stub
     //   return name.hashCode()*37+age;
    //}
}
 
public class Main {
 
    public static void main(String[] args) {
 
        People p1 = new People("Jack", 12);
        System.out.println(p1.hashCode());
 
        HashMap<People, Integer> hashMap = new HashMap<People, Integer>();
        hashMap.put(p1, 1);
 
        System.out.println(hashMap.get(new People("Jack", 12)));//这里是new的新对象
    }
}
```

例如在哈希表中存储数据,如果要存储的类型只重写equals,这样当两个被判定为相等的对象,但是他们两个的存储地址肯定不同,所以在取值时肯定为null,

所以需要重写hashcode方法,让equals方法和hashcode方法始终在逻辑上保持一致性



### int,long的取值范围以及BigDecimal，数值越界了如何处理？

转换为更高级别或BigDecimal

### int、char、long各占多少字节数

http://www.importnew.com/18346.html

### int与integer的区别

int是基本类型,Integer是int的包装类

Integer也必须实例化才能使用(表面上是自动装箱)



### string 转换成 integer的方式及原理

integer.parseInt(string str)方法调用Integer内部的 
parseInt(string str,10)方法,默认基数为10，parseInt内部首先 
判断字符串是否包含符号（-或者+），则对相应的negative和limit进行 
赋值，然后再循环字符串，对单个char进行数值计算Character.digit(char ch, int radix) 
在这个方法中，函数肯定进入到0-9字符的判断（相对于string转换到int）， 否则会抛出异常，数字就是如上面进行拼接然后生成的int类型数值

### 讲一下常见编码方式

http://www.ruanyifeng.com/blog/2007/10/ascii_unicode_and_utf-8.html

ASCII GBK unicode

### utf-8编码中的中文占几个字节(两个char,4)；int型几个字节(4)

UTF-8 是 Unicode 的实现方式之一



内部类和静态内部类和匿名内部类，以及项目中的应用

静态内部类的设计意图

同步普通方法跟static方法之间的区别

### 父类的静态方法能否被子类重写(不能)

### 静态属性和静态方法是否可以被继承？是否可以被重写？以及原因？

静态方法不跟所属类相关联,不需要通过创建对象来调用,所以就谈不上重写,

重写的目的在于根据创建对象的所属类型不同而表现出多态

### 静态代理和动态代理的区别，什么场景使用



### 抽象类和接口的区别

http://www.importnew.com/12399.html

最新版JAVA当中,抽象类和接口已经接近相同了,当单继承可以考虑抽象类,多继承用接口



### Java为何引入泛型，泛型边界

http://www.importnew.com/19740.html

为了编译时代码类型不出错

请记住PECS原则：生产者（Producer）使用extends，消费者（Consumer）使用super



### 注解如何获取

### 反射为何耗性能

getMethod和getDeclaredField方法会比invoke和set方法耗时；

反射包括了一些动态类型，所以 JVM 无法对这些代码进行优化。因此，反射操作的效 
率要比那些非反射操作低得多。我们应该避免在经常被 执行的代码或对性能要求很高的程 
序中使用反射。



### 类的初始化过程

https://blog.csdn.net/w1196726224/article/details/56529615

类的初始化顺序依次是（静态变量、静态代码块）>（变量、代码块）>构造方法

### 不用锁如何保证int自增安全

使用AtomicInteger类

### 深拷贝浅拷贝

实则浅拷贝和深拷贝只是相对的，如果一个对象内部只有基本数据类型，那用 clone() 方法获取到的就是这个对象的深拷贝，而如果其内部还有引用数据类型，那用 clone() 方法就是一次浅拷贝的操作。

### 谈谈对java多态的理解

https://www.cnblogs.com/chenssy/p/3372798.html

### final，finally，finalize的区别

final       用于申明属性，方法和类，表示属性不可变，方法不可以被覆盖，类不可以被继承。
finally     是异常处理语句结构中，表示总是执行的部分。　　

finallize   表示是object类一个方法，在垃圾回收机制中执行的时候会被调用被回收对象的方法。允许回收此前未回收的内存垃圾。所有object都继承了finalize（）方法

### Java的异常体系

https://blog.csdn.net/liuhenghui5201/article/details/18675391

1、 异常是程序运行过程过程出现的错误，在Java中用类来描述，用对象来表示具体的异常。 
        Java将其区分为Error与Exception，Error是程序无力处理的错误，Exception是程序可以处理的错误。 
        异常处理是为了程序的健壮性。 
2、 Java异常类来自于Java API定义和用户扩展。通过继承Java API异常类可以实现异常的转译。 
3、 异常能处理就处理，不能处理就抛出，最终没有处理的异常JVM会进行处理。 
4、 异常可以传播，也可以相互转译，但应该根据需要选择合理的异常转译的方向。 
5、 对于一个应用系统，设计一套良好的异常处理体系很重要。这一点在系统设计的时候就应该考虑到。

### NIO

### Resume

https://github.com/Trinea/trinea-download/tree/master/resume