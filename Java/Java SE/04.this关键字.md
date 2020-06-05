# this关键字的作用

* this代表了当前对象的引用
* this关键字可以用在实例方法和构造器中
* this用在方法中，谁调用这个方法，this就代表谁
* this用在构造器中，代表构造器正在初始化的那个对象的引用

# 1. 在方法中引用调用该方法的对象

this关键字最大的作用就是**让类中的一个方法访问该类里的另一个方法或者实例变量**。

假设定义了一个Person类，这个Person对象的eat()方法需要调用它的move()方法，则如何做呢？是否应该定义如下的Person类呢？

```java
public class Person {
    //定义一个move()方法
    public void move(){
        System.out.println("正在执行move()方法");
    }
    //定义一个eat()方法，eat()方法需要借助move()方法
    public void eat(){
        Person p = new Person();
        p.move();
        System.out.println("正在执行eat()方法");
    }
    public static void main(String[] args) {
        //创建Person对象
        Person p = new Person();
        //调用Person的eat()方法
        p.eat();
    }
}
```

运行结果为：

```bash
正在执行move()方法
正在执行eat()方法
```

以上这种方式确实能够做到在eat()方法里调用move()方法，但从main()方法里的程序中可以看出，一共创建了两个对象：main()方法里创建一个对象；eat()方法里创建一个对象。可是真的需要创建两个对象吗？答案是否定的！因为当程序调用eat()方法时一定会提供一个Person对象，而不需要重新创建一个Person对象了。

因此需要在eat()方法中获得调用该方法的对象，通过this关键字就可以满足这个需求。

**this可以代表任何对象，当this出现在某个方法体中时，它所代表的对象是不确定的，但它的类型是确定的，它所代表的类型只能是当前类。只有当这个方法被调用时，它所代表的对象才被确定下来：谁在调用这个方法，this就代表谁。**

将上面的Person类中的eat()方法改为一下这种方式更合适：

```java
//定义一个eat()方法，eat()方法需要借助move()方法
public void eat(){
    //使用this引用调用eat()方法的对象
    this.move();
    System.out.println("正在执行eat()方法");
}
```

上述程序中eat()方法需要依赖于move()方法，现实中这种依赖情形非常常见，例如写字方法需要拿笔的方法，这种依赖都是同一个对象两个方法之间的依赖。因此，Java允许对象的的一个成员直接调用另一成员，可以省略this前缀。也就是说，上面的程序可以改为如下形式：

```java
public void eat(){
    move();
    System.out.println("正在执行eat()方法");
}
```

# 2. 构造器中引用该构造器正在初始化的对象

this关键字可用于构造器中作为默认引用，由于构造器是直接使用new关键字来调用，而不是使用对象来调用的，所以this在构造器中代表该构造器正在初始化的对象。例如下面的程序：

```java
public class Person {
    //定义一个名为age的成员变量
    public int age;
    //构造器
    public Person() {
        //在构造器里定义一个age变量
        int age = 0;
        //使用this代表该构造器正在初始化的对象
        //下面的代码将会把该构造器正在初始化的对象的age成员变量设为3
        this.age = 3;
    }
    public static void main(String[] args) {
        //使用new Person()创建的对象的age成员变量都将被设为3
        //下面代码输出3
        System.out.println(new Person().age);
    }
}
```

与普通方法类似的是，大部分时候，在构造器中访问其它成员变量和方法时都可以省略this前缀，但如果构造器中有一个与成员变量同名的局部变量，又必须在构造器中访问这个被覆盖的成员变量，则必须使用this前缀。如上面程序所示。

# 3. 返回对象的值

当this作为对象的默认引用使用时，程序可以像访问普通引用变量一样来访问这个this引用，甚至可以把this当成普通方法的返回值。请看下面程序：

```java
public class Person {
    public int age;
    public Person grow() {
        age ++;
        return this;
    }
    public static void main(String[] args) {
        Person p = new Person();
        //可以连续调用同一个方法
        p.grow().grow().grow();
        System.out.println("p对象的age的值是："+p.age);
    }
}
```

运行结果为：

```text
p对象的age的值是：3
```

从上面的程序可以看出，如果在某个方法中把this作为返回值，则可以多次连续调用同一个方法，从而使得代码更加的简洁。但这种方式容易造成实际意义的模糊，例如上面的group()方法，用于表示对象的生长，即age变量的值加1，实际上不应该有返回值。

最后需要强调一点：静态成员不能直接访问非静态成员，即static修饰的方法不能访问不适用static修饰的普通方法。对于static修饰的方法而言，可以使用类直接调用该方法，如果在static修饰的方法中使用this关键字，则这个关键字就无法指向合适的对象。所以，static修饰的方法中不能使用this引用。