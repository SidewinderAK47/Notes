# Java 静态块浅析

（整理转载自知乎用户： [happybean](https://www.zhihu.com/people/happybean-4) ）：链接：https://zhuanlan.zhihu.com/p/45107365

 静态块，形式为“static{...}”，静态块里的内容在类被加载的时候就执行，存在方法区（静态区）中，能被线程共享。

 

```Java
package com.wxd;

public class StaticDemo {
    public static void main(String[] args) {
        try {
            Class.forName("com.wxd.Demo1");
            Class.forName("com.wxd.Demo1");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

class Demo1 {
    static {
        System.out.println("Demo1 static block !");
    }
}
// 结果： Demo1 static block !
```

static代码块执行时间：

1.类第一次创建对象：

```Java
　new StaticDemo();    
```

 2.调用该类的静态方法时（静态块代码 要比 静态函数先执行）。 

```java
package com.wxd;

public class StaticDemo {
    public static void main(String[] args) {
        //Demo1 demo = new Demo1();
        Demo1.StaticMethod();
    }
}

class Demo1 {
    static {
        System.out.println("Demo1 static block !");
    }

    public static void StaticMethod() {
        System.out.println("Static Method !");
    }
}
//执行结果：
//Demo1 static block ! 
//Static Method !
```

3.使用类的非静态常量。

```java
public class StaticDemo {
    public static void main(String[] args) {
        Demo2.a =2;
    }
}
class Demo2 {
    public static int a;
    static {
        System.out.println("Demo2 static block !");
    }
}
```

4.使用反射加载类对象

```Java
public class StaticDemo {
    public static void main(String[] args) {
        try {
            Class.forName("com.wxd.Demo1");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}

class Demo1 {
    static {
        System.out.println("Demo1 static block !");
    }
    public static void StaticMethod() {
        System.out.println("Static Method !");
    }
}
//Demo1 static block !
```

 5.初始化该类的子类（首先执行父类静态内容，然后执行子类静态内容，然后依次执行父类构造函数，子类构造函数） 

```Java
package com.wxd;

public class StaticDemo {
    public static void main(String[] args) {
        //Demo1 demo = new Demo1();
        //Demo1.StaticMethod();
        //Demo2.a =2;
        new Demo2();
    }
}

class Demo1 {
    static {
        System.out.println("Demo1 static block !");
    }
    Demo1() {
        System.out.println("Demo1 !");
    }
}
class Demo2 extends  Demo1 {
    static {
        System.out.println("Demo2 static block !");
    }
    Demo2() {
        System.out.println("Demo2 !");
    }
}
//执行结果：
//Demo1 static block !
//Demo2 static block !
//Demo1 !
//Demo2 !
```

当一类有多个静态代码块（按从上往下顺序执行）；

```Java
public class StaticDemo {
    public static void main(String[] args) {
        new Demo1();
    }
}

class Demo1 {
    static {
        System.out.println("Demo1 static block1 !");
    }
    static {
        System.out.println("Demo1 static block2 !");
    }
    static {
        System.out.println("Demo1 static block3 !");
    }
    Demo1() {
        System.out.println("Demo1 !");
    }
    static {
        System.out.println("Demo1 static block4 !");
    }
}
//执行结果：
//Demo1 static block1 !
//Demo1 static block2 !
//Demo1 static block3 !
//Demo1 static block4 !
//Demo1 !
```

