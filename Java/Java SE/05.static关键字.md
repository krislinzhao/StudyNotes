# 1. 什么是static关键字

​	在Java中static表示“全局”或者“静态”的意思，用来修饰成员变量和成员方法，当然也可以修饰代码块。

​     Java把内存分为栈内存和堆内存，其中栈内存用来存放一些基本类型的变量、数组和对象的引用，堆内存主要存放一些对象。在JVM加载一个类的时候，若该类存在static修饰的成员变量和成员方法，则会为这些成员变量和成员方法在固定的位置开辟一个固定大小的内存区域（只要这个类被加载，Java虚拟机就能根据类名在运行时数据区的方法区内定找到他们），有了这些“固定”的特性，那么JVM就可以非常方便地访问他们。

​      同时被static修饰的成员变量和成员方法是独立于该类的，它不依赖于某个特定的实例变量，也就是说它被该类的所有实例共享。所有实例的引用都指向同一个地方，任何一个实例对其的修改都会导致其他实例的变化。

```java
public class User {
    private static int userNumber  = 0 ;
    
    public User(){
        userNumber ++;
    }
    
    public static void main(String[] args) {
        User user1 = new User();
        User user2 = new User();
        
        System.out.println("user1 userNumber：" + User.userNumber);
        System.out.println("user2 userNumber：" + User.userNumber);
    }
}  
```

输出：

```
user1 userNumber：2
user2 userNumber：2
```

# 2. static关键字的用途

## 2.1 static变量

　static变量也称作静态变量，静态变量和非静态变量的区别是：静态变量被所有的对象所共享，在内存中只有一个副本，它当且仅当在类初次加载时会被初始化。而非静态变量是对象所拥有的，在创建对象的时候被初始化，存在多个副本，各个对象拥有的副本互不影响。

　static成员变量的初始化顺序按照定义的顺序进行初始化。

​	所以我们一般在这两种情况下使用静态变量：对象之间共享数据、访问方便。

```java
public class TestStatic {
    
    public static int count = 0;
    
    public static void main(String[] args){
        TestStatic test1=new TestStatic();
        System.out.println(test1.count);
        TestStatic test2=new TestStatic();
        test2.count++;
        System.out.println(test1.count+" "+test2.count+" "+TestStatic.count);
    }
}
```

输出结果:

```
0
1 1 1
```

​	可见，static变量并不是所在类的某个具体对象所有，而是该类的所有对象所共有的，静态变量既能被对象调用，也能直接拿类来调用。

## 2.2 static方法

​	static方法一般称作静态方法，由于静态方法不依赖于任何对象就可以进行访问，因此对于静态方法来说，是没有this的，因为它不依附于任何对象，既然都没有对象，就谈不上this了。并且由于这个特性，在静态方法中不能访问类的非静态成员变量和非静态成员方法，因为非静态成员方法/变量都是必须依赖具体的对象才能够被调用。

​	但是要注意的是，虽然在静态方法中不能访问非静态成员方法和非静态成员变量，但是在非静态成员方法中是可以访问静态成员方法/变量的。
​     因为static方法独立于任何实例，因此static方法必须被实现，而不能是抽象的abstract。

总结一下，对于静态方法需要注意以下几点：
（1）它们仅能调用其他的static 方法。
（2）它们只能访问static数据。
（3）它们不能以任何方式引用this 或super。

举个简单的例子：


![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200518155532.png)

　　在上面的代码中，由于print2方法是独立于对象存在的，可以直接用过类名调用。假如说可以在静态方法中访问非静态方法/变量的话，那么如果在main方法中有下面一条语句：

　　MyObject.print2();

　　此时对象都没有，str2根本就不存在，所以就会产生矛盾了。同样对于方法也是一样，由于你无法预知在print1方法中是否访问了非静态成员变量，所以也禁止在静态成员方法中访问非静态成员方法。

　　而对于非静态成员方法，它访问静态成员方法/变量显然是毫无限制的。

　　因此，如果说想在不创建对象的情况下调用某个方法，就可以将这个方法设置为static。我们最常见的static方法就是main方法，至于为什么main方法必须是static的，现在就很清楚了。因为程序在执行main方法的时候没有创建任何对象，因此只有通过类名来访问。

## 2.3 static代码块

　　static关键字还有一个比较关键的作用就是 用来形成静态代码块以优化程序性能。static块可以置于类中的任何地方，类中可以有多个static块。在类初次被加载的时候，会按照static块的顺序来执行每个static块，并且只会执行一次。

　　为什么说static块可以用来优化程序性能，是因为它的特性:只会在类加载的时候执行一次。下面看个例子:

```java
class Person{ 
    private Date birthDate; 
      
    public Person(Date birthDate) { 
        this.birthDate = birthDate; 
    } 
      
    boolean isBornBoomer() { 
        Date startDate = Date.valueOf("1946"); 
        Date endDate = Date.valueOf("1964"); 
        return birthDate.compareTo(startDate)>=0 && birthDate.compareTo(endDate) < 0; 
    } 
} 
```

　　isBornBoomer是用来这个人是否是1946-1964年出生的，而每次isBornBoomer被调用的时候，都会生成startDate和birthDate两个对象，造成了空间浪费，如果改成这样效率会更好：

```java
class Person{ 
    private Date birthDate; 
    private static Date startDate,endDate; 
    static{ 
        startDate = Date.valueOf("1946"); 
        endDate = Date.valueOf("1964"); 
    } 
      
    public Person(Date birthDate) { 
        this.birthDate = birthDate; 
    } 
      
    boolean isBornBoomer() { 
        return birthDate.compareTo(startDate)>=0 && birthDate.compareTo(endDate) < 0; 
    } 
} 
```

　　因此，很多时候会将一些只需要进行一次的初始化操作都放在static代码块中进行。

# 3. static关键字的误区

## 3.1.static关键字会改变类中成员的访问权限吗？

　　有些初学的朋友会将java中的static与C/C++中的static关键字的功能混淆了。在这里只需要记住一点：与C/C++中的static不同，Java中的static关键字不会影响到变量或者方法的作用域。在Java中能够影响到访问权限的只有private、public、protected（包括包访问权限）这几个关键字。看下面的例子就明白了：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200518160541.png)

　提示错误"age是私有的 "，这说明static关键字并不会改变变量和方法的访问权限。

## 3.2 能通过this访问静态成员变量吗？

　　虽然对于静态方法来说没有this，那么在非静态方法中能够通过this访问静态成员变量吗？先看下面的一个例子，这段代码输出的结果是什么？

```java
public class Main {　　 
    static int value = 33;  
  
    public static void main(String[] args) throws Exception{ 
        new Main().printValue(); 
    } 
  
    private void printValue(){ 
        int value = 3; 
        System.out.println(this.value); 
    } 
} 
```

 输出结果：33

　　这里面主要考察队this和static的理解。this代表什么？this代表当前对象，那么通过new Main()来调用printValue的话，当前对象就是通过new Main()生成的对象。而static变量是被对象所享有的，因此在printValue中的this.value的值毫无疑问是33。在printValue方法内部的value是局部变量，根本不可能与this关联，所以输出结果是33。在这里永远要记住一点：静态成员变量虽然独立于对象，但是不代表不可以通过对象去访问，所有的静态方法和静态变量都可以通过对象访问（只要访问权限足够）。

## 3.3 **static能作用于局部变量么？**

　　在C/C++中static是可以作用域局部变量的，但是在Java中切记：static是不允许用来修饰局部变量。不要问为什么，这是Java语法的规定。

## 3.4 **static和final一块用表示什么？**
​      static final用来修饰成员变量和成员方法，可简单理解为“全局常量”！ 

​	对于变量，表示一旦给值就不可修改，并且通过类名可以访问。 

​	对于方法，表示不可覆盖，并且可以通过类名直接访问。

# 4. 常见的笔试面试题

## 4.1 第一题

```java
public class Test extends Base{ 
  
    static{ 
        System.out.println("test static"); 
    } 
      
    public Test(){ 
        System.out.println("test constructor"); 
    } 
      
    public static void main(String[] args) { 
        new Test(); 
    } 
} 
  
class Base{ 
      
    static{ 
        System.out.println("base static"); 
    } 
      
    public Base(){ 
        System.out.println("base constructor"); 
    } 
} 
```

输出结果：

```
base static
test static
base constructor
test constructor
```

　　这段代码具体的执行过程，在执行开始，先要寻找到main方法，因为main方法是程序的入口，但是在执行main方法之前，必须先加载Test类，而在加载Test类的时候发现Test类继承自Base类，因此会转去先加载Base类，在加载Base类的时候，发现有static块，便执行了static块。在Base类加载完成之后，便继续加载Test类，然后发现Test类中也有static块，便执行static块。在加载完所需的类之后，便开始执行main方法。在main方法中执行new Test()的时候会先调用父类的构造器，然后再调用自身的构造器。因此，便出现了上面的输出结果。

## 4.2 第二题

```java
public class Test { 
    Person person = new Person("Test"); 
    static{ 
        System.out.println("test static"); 
    } 
      
    public Test() { 
        System.out.println("test constructor"); 
    } 
      
    public static void main(String[] args) { 
        new MyClass(); 
    } 
} 
  
class Person{ 
    static{ 
        System.out.println("person static"); 
    } 
    public Person(String str) { 
        System.out.println("person "+str); 
    } 
} 
  
  
class MyClass extends Test { 
    Person person = new Person("MyClass"); 
    static{ 
        System.out.println("myclass static"); 
    } 
      
    public MyClass() { 
        System.out.println("myclass constructor"); 
    } 
} 
```

输出结果：

```
test static
myclass static
person static
person Test
test constructor
person MyClass
myclass constructor
```

　　这段代码的具体执行过程：首先加载Test类，因此会执行Test类中的static块。接着执行new MyClass()，而MyClass类还没有被加载，因此需要加载MyClass类。在加载MyClass类的时候，发现MyClass类继承自Test类，但是由于Test类已经被加载了，所以只需要加载MyClass类，那么就会执行MyClass类的中的static块。在加载完之后，就通过构造器来生成对象。而在生成对象的时候，必须先初始化父类的成员变量，因此会执行Test中的Person person = new Person()，而Person类还没有被加载过，因此会先加载Person类并执行Person类中的static块，接着执行父类的构造器，完成了父类的初始化，然后就来初始化自身了，因此会接着执行MyClass中的Person person = new Person()，最后执行MyClass的构造器。

## 4.3 第三题

```java
public class Test {
     
    static{
        System.out.println("test static 1");
    }
    public static void main(String[] args) {
         
    }
     
    static{
        System.out.println("test static 2");
    }
}
```

输出结果：

```
test static 1
test static 2
```

　　虽然在main方法中没有任何语句，但是还是会输出，原因上面已经讲述过了。另外，static块可以出现类中的任何地方（只要不是方法内部，记住，任何方法内部都不行），并且执行是按照static块的顺序执行的。

# 5. 静态代码块的初始化顺序

```java
class Parent {
    static String name = "hello";
    {
        System.out.println("parent block");
    }
    static {
        System.out.println("parent static block");
    }

    public Parent() {
        System.out.println("parent constructor");
    }
}

class Child extends Parent {
    static String childName = "hello";
    {
        System.out.println("child block");
    }
    static {
        System.out.println("child static block");
    }

    public Child() {
        System.out.println("child constructor");
    }
}

public class TestStatic {

    public static void main(String[] args) {
        new Child();// 语句(*)
    }
}
```

输出结果：

```
parent static block
child static block
parent block
parent constructor
child block
child constructor
```

分析:当执行new Child()时，它首先去看父类里面有没有静态代码块，如果有，它先去执行父类里面静态代码块里面的内容，当父类的静态代码块里面的内容执行完毕之后，接着去执行子类(自己这个类)里面的静态代码块，当子类的静态代码块执行完毕之后，它接着又去看父类有没有非静态代码块，如果有就执行父类的非静态代码块，父类的非静态代码块执行完毕，接着执行父类的构造方法；父类的构造方法执行完毕之后，它接着去看子类有没有非静态代码块，如果有就执行子类的非静态代码块。子类的非静态代码块执行完毕再去执行子类的构造方法，这个就是一个对象的初始化顺序。

# 总结
对象的初始化顺序:**首先**执行父类静态的内容，父类静态的内容执行完毕后，**接着**去执行子类的静态的内容，当子类的静态内容执行完毕之后，**再去**看父类有没有非静态代码块，如果有就执行父类的非静态代码块，父类的非静态代码块执行完毕，**接着**执行父类的构造方法；父类的构造方法执行完毕之后，它**接着**去看子类有没有非静态代码块，如果有就执行子类的非静态代码块。子类的非静态代码块执行完毕**再去**执行子类的构造方法。

总之一句话，**静态代码块内容先执行，接着执行父类非静态代码块和构造方法，然后执行子类非静态代码块和构造方法。**

# 注意

子类的构造方法，不管这个构造方法带不带参数，默认的它都会先去寻找父类的不带参数的构造方法。如果父类没有不带参数的构造方法，那么子类必须用supper关键子来调用父类带参数的构造方法，否则编译不能通过。



# A. static关键字使用场景

## 1. 静态变量

　把一个变量声明为静态变量通常基于以下三个目的：

- **作为共享变量使用**
- **减少对象的创建**
- **保留唯一副本**

　　第一种比较容易理解，由于static变量在内存中只会存在一个副本，所以其可以作为共享变量使用，比如要定义一个全局配置、进行全局计数。如：

```java
public class CarConstants {
　　// 全局配置,一般全局配置会和final一起配合使用, 作为共享变量
　　public static final int MAX_CAR_NUM = 10000; 
}
 
public class CarFactory {
　　// 计数器
　　private static int createCarNum = 0;
     
    public static Car createCar() {
      if (createCarNum > CarConstants.MAX_CAR_NUM) {
        throw new RuntimeException("超出最大可生产数量");
      }
      Car c = new Car();
      createCarNum++;
      return c;
    }
   
    public static getCreateCarNum() {
      return createCarNum;
    }
}
```

​	第二种虽然场景不多，但是基本在每个工程里面都会使用到，比如声明Loggger变量：

```java
private static final Logger LOGGER = LogFactory.getLoggger(MyClass.class);
```

​	实际上，如果把static去掉也是可行的，比如：

```java
private final Logger LOGGER = LogFactory.getLoggger(MyClass.class);
```

　　这样一来，对于每个MyClass的实例化对象都会拥有一个LOGGER，如果创建了1000个MyClass对象，则会多出1000个Logger对象，造成资源的浪费，因此通常会将Logger对象声明为static变量，这样一来，能够减少对内存资源的占用。

　　第三种最经典的场景莫过于单例模式了，单例模式由于必须全局只保留一个副本，所以天然和static的初衷是吻合的，用static来修饰再合适不过了。

```java
public class Singleton {
    private static volatile Singleton singleton;
 
    private Singleton() {}
 
    public static Singleton getInstance() {
        if (singleton == null) {
            synchronized (Singleton.class) {
                if (singleton == null) {
                    singleton = new Singleton();
                }
            }
        }
        return singleton;
    }
}
```

## 2. 静态方法

　　将一个方法声明为静态方法，通常是**为了方便在不创建对象的情况下调用**。这种使用方式非常地常见，比如jdk的Collections类中的一些方法、单例模式的getInstance方法、工厂模式的create/build方法、util工具类中的方法。

## 3. 静态代码块

　　静态代码块通常来说是为了对静态变量进行一些初始化操作，比如单例模式、定义枚举类：

- 单例模式

  ```java
  public class Singleton {
      private static Singleton instance;
   
      static {
          instance = new Singleton();
      }
   
      private Singleton() {}
   
      public static Singleton getInstance() {
          return instance;
      }
  }
  ```

* 枚举类

  ```java
  public enum WeekDayEnum {
      MONDAY(1,"周一"),
      TUESDAY(2, "周二"),
      WEDNESDAY(3, "周三"),
      THURSDAY(4, "周四"),
      FRIDAY(5, "周五"),
      SATURDAY(6, "周六"),
      SUNDAY(7, "周日");
   
      private int code;
      private String desc;
   
      WeekDayEnum(int code, String desc) {
          this.code = code;
          this.desc = desc;
      }
   
      private static final Map<Integer, WeekDayEnum> WEEK_ENUM_MAP = new HashMap<Integer, WeekDayEnum>();
   
      // 对map进行初始化
      static {
          for (WeekDayEnum weekDay : WeekDayEnum.values()) {
              WEEK_ENUM_MAP.put(weekDay.getCode(), weekDay);
          }
      }
   
      public static WeekDayEnum findByCode(int code) {
          return WEEK_ENUM_MAP.get(code);
      }
   
      public int getCode() {
          return code;
      }
   
      public void setCode(int code) {
          this.code = code;
      }
   
      public String getDesc() {
          return desc;
      }
   
      public void setDesc(String desc) {
          this.desc = desc;
      }
  }　
  ```

## 4. 静态内部类

  　　内部类一般情况下使用不是特别多，如果需要在外部类里面定义一个内部类，通常是基于外部类和内部类有很强关联的前提下才去这么使用。

  　　在说静态内部类的使用场景之前，我们先来看一下静态内部类和非静态内部类的区别：

  　　**非静态内部类对象持有外部类对象的引用（编译器会隐式地将外部类对象的引用作为内部类的构造器参数）；而静态内部类对象不会持有外部类对象的引用**

  　　由于非静态内部类的实例创建需要有外部类对象的引用，所以非静态内部类对象的创建必须依托于外部类的实例；而静态内部类的实例创建只需依托外部类；

  　　并且由于非静态内部类对象持有了外部类对象的引用，因此非静态内部类可以访问外部类的非静态成员；而静态内部类只能访问外部类的静态成员；

   

  　　两者的根本性区别其实也决定了用static去修饰内部类的真正意图：

  - 内部类需要脱离外部类对象来创建实例
  - 避免内部类使用过程中出现内存溢出

  　　

  第一种是目前静态内部类使用比较多的场景，比如JDK集合中的Entry、builder设计模式。

　　HashMap Entry：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200518164800.png)

　　builder设计模式：

```java
public class Person {
    private String name;
    private int age;
 
    private Person(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
    }
 
    public static class Builder {
 
        private String name;
        private int age;
 
        public Builder() {
        }
 
        public Builder name(String name) {
            this.name = name;
            return this;
        }
        public Builder age(int age) {
            this.age=age;
            return this;
        }
 
        public Person build() {
            return new Person(this);
        }
    }
 
    public String getName() {
        return name;
    }
 
    public void setName(String name) {
        this.name = name;
    }
 
    public int getAge() {
        return age;
    }
 
    public void setAge(int age) {
        this.age = age;
    }
}
 
// 在需要创建Person对象的时候
Person person = new Person.Builder().name("张三").age(17).build();
```

第二种情况一般出现在多线程场景下，非静态内部类可能会引发内存溢出的问题，比如下面的例子：

```java
public class Task {
 
    public void onCreate() {
        // 匿名内部类, 会持有Task实例的引用
        new Thread() {
            public void run() {
                //...耗时操作
            };
        }.start();   
    }
}
```

　上面这段代码中的：

```java
new Thread() {
  public void run() {
  //...耗时操作
  };
}.start();
```

　　声明并创建了一个匿名内部类对象，该对象持有外部类Task实例的引用，如果在在run方法中做的是耗时操作，将会导致外部类Task的实例迟迟不能被回收，如果Task对象创建过多，会引发内存溢出。

​	优化方式：

```java
public class Task {
 
    public void onCreate() {
        SubTask subTask = new SubTask();
        subTask.start();
    }
     
    static class SubTask extends Thread {
        @Override
        public void run() {
            //...耗时操作   
        }
         
    }
}
```

## 5. **静态导入**

静态导入其实就是import static，用来导入某个类或者某个包中的静态方法或者静态变量。如下面这段代码所示：

```java
import static java.lang.Math.PI;
  
    public  class MathUtils {
 
    public static double calCircleArea(double r) {
        // 可以直接用 Math类中的静态变量PI
        return PI * r * r;
    }
}
```

　这样在书写代码的时候确实能省一点代码，但是会影响代码可读性，所以一般情况下不建议这么使用。

# B. static变量和普通成员变量区别

　static变量和普通成员变量主要有以下4点区别：

- 区别1：所属不同。static变量属于类，不单属于任何对象；普通成员变量属于某个对象
- 区别2：存储区域不同。static变量位于方法区；普通成员变量位于堆区。
- 区别3：生命周期不同。static变量生命周期与类的生命周期相同；普通成员变量和其所属的对象的生命周期相同。
- 区别4：在对象序列化时（Serializable），static变量会被排除在外（因为static变量是属于类的，不属于对象）

# C. 类的构造器到底是不是static方法？

　　关于类的构造器是否是static方法有很多争议，在《java编程思想》一书中提到“类的构造器虽然没有用static修饰，但是实际上是static方法”，个人认为这种说法有点欠妥，原因如下：

　　1）在类的构造器中，实际上有一个隐藏的参数this引用，this是跟对象绑定的，也就是说在调用构造器之前，这个对象已经创建完毕了才能出现this引用。而构造器的作用是干什么的呢？它负责在创建一个实例对象的时候对实例进行初始化操作，即jvm在堆上为实例对象分配了相应的存储空间后，需要调用构造器对实例对象的成员变量进行初始化赋值操作。

　　2）我们再来看static方法，由于static不依赖于任何对象就可以进行访问，也就是说和this是没有任何关联的。从这一层面去讲，类的构造器不是static方法

　3）从JVM指令层面去看，类的构造器不是static方法，我们先看一下下面这段代码：

```java
class Person {
  private String name;
   
  public Person(String name) {
    this.name = name;
  }
   
  public static void create() {
     
  }
}
 
 
public class Main {
  public static void main(String[] args) {
    Person.create();
    Person p = new Person("Jack");
  }
}
```

　　这段代码反编译之后的字节码如下：

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200518165557.png)

　　从上面可以看出，在调用static方法是调用的是invokestatic指令，而在调用类的构造器时实际上执行的是invokespecial指令，而这2个指令在JVM规范中的解释如下：

　　https://docs.oracle.com/javase/specs/jvms/se7/html/jvms-6.html#jvms-6.5.invokestatic

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200518165632.png)

![](https://gitee.com/krislin_zhao/IMGcloud/raw/master/img/20200518165653.png)

　　可以看出，这2个指令的用途是完全不同的，invokestatic定义很清楚，就是用来调用执行static方法，而invokespecial用来调用实例方法，用来特殊调用父类方法、private方法和类的构造器。