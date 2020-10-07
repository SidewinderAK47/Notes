

![img](Object类分析.assets/v2-c2b74da4856f934fc1eba65d025a9da7_b.jpg)

```java
package java.lang;

public class Object {

    
    private static native void registerNatives();
    static {
        registerNatives();
    }
    
    public final native Class<?> getClass();


    public native int hashCode();


    public boolean equals(Object obj) {
        return (this == obj);
    }


    protected native Object clone() throws CloneNotSupportedException;


    public String toString() {
        return getClass().getName() + "@" + Integer.toHexString(hashCode());
    }

    public final native void notify();

    public final native void notifyAll();

    public final native void wait(long timeout) throws InterruptedException;

    
    public final void wait(long timeout, int nanos) throws InterruptedException {
        if (timeout < 0) {
            throw new IllegalArgumentException("timeout value is negative");
        }

        if (nanos < 0 || nanos > 999999) {
            throw new IllegalArgumentException(
                                "nanosecond timeout value out of range");
        }

        if (nanos > 0) {
            timeout++;
        }

        wait(timeout);
    }

    public final void wait() throws InterruptedException {
        wait(0);
    }

    protected void finalize() throws Throwable { }
}

```



其实就可以归纳成几个：

- `registerNatives()`【底层实现、不研究】
- `hashCode()`       HashMap用于
- `equals(Object obj)`   
- `clone()`  克隆对象
- `toString() `
- `notify()`
- `notifyAll()`
- `wait(long timeout)`【还有重载了两个】
- `finalize()`

Object一共有**11**个方法，其中一个为底层的实现`registerNatives()`，其中两个`wait()`和`wait(long timeout, int nanos)`重载方法。

- 所以我们真正需要看的就是**8个**方法

还有**一个属性**：

```java
 public final native Class<?> getClass();
```

`getClass()` 返回一个类字节码对象；

`notify()` 



看完上面的注释我们可以**总结以下的要点**：

- 无论是wait、notify还是notifyAll()都需要**由监听器对象(锁对象)来进行调用**

- - 简单来说：**他们都是在同步代码块中调用的**，否则会抛出异常！

- `notify()`唤醒的是在等待队列的**某个**线程(不确定会唤醒哪个)，`notifyAll()`唤醒的是等待队列**所有**线程

- 导致`wait()`的线程被唤醒可以有4种情况

- - 该线程被中断
  - `wait()`时间到了
  - 被`notify()`唤醒
  - 被`notifyAll()`唤醒

- 调用`wait()`的线程会**释放掉锁**

其实总结完上面的并不会有比较深刻的印象，可以尝试着回答几个问题来加深对`wait()`和`notify()`的理解。



### Wait方法

```text
public final void wait()
                throws InterruptedException
```

>  在**其他线程**,调用**此对象**的 notify() 方法或 notifyAll()  方法前，导致当前线程等待。

换句话说，此方法的行为就好像它仅执行 wait(0) 调用一样。 



当前线程必须拥有此**对象监视器**。

该线程发布对此**监视器**的所有权并等待，直到其他线程通过调用 notify 方法，或  notifyAll 方法通知在此对象的监视器上等待的线程醒来。然后该线程将等到重新获得对监视器的所有权后才能继续执行。 

对于某一个参数的版本，实现中断和虚假唤醒是可能的，而且此方法应始终在循环中使用： 

```text
synchronized (obj) {
while (<condition does not hold>)
obj.wait();
... // Perform action appropriate to condition
     }
 
```

此方法只应由作为此**对象监视器的所有者的线程**来调用。

有关线程能够成为监视器所有者的方法的描述，请参阅 notify 方法。
**抛出：**
IllegalMonitorStateException  - **如果当前线程不是此对象监视器的所有者。**

InterruptedException  - 如果在当前线程等待通知之前或者正在等待通知时，任何线程中断了当前线程。在抛出此异常时，当前线程的中断状态被清除。



## 为什么wait和notify在Object方法上？

从一开始我们就说了：`wait()`和`notify()`是Java给我们提供线程之间通信的API，既然是线程的东西，那什么是在Object类上定义，而不是在Thread类上定义呢？

因为我们的**锁是对象锁**【要是忘记的同学可回顾：[Java锁机制了解一下](https://link.zhihu.com/?target=https%3A//link.juejin.im/%3Ftarget%3Dhttps%3A%2F%2Fmp.weixin.qq.com%2Fs%3F__biz%3DMzI4Njg5MDA5NA%3D%3D%26mid%3D2247484198%26idx%3D1%26sn%3D4d8e372165bb49987a6243f17153a9b4%26chksm%3Debd74227dca0cb31311886f835092c9360d08a9f0a249ece34d4b1e49a31c9ec773fa66c8acc%23rd)】，每个对象都可以成为锁。**让当前线程等待某个对象的锁，当然应该通过这个对象来操作了**。

- 锁对象是**任意**的，所以这些方法必须定义在Object类中



## notify方法调用后，会发生什么？

上面已经说了，notify会唤醒某个处于等待队列的线程。

但是要**注意**的是：

- notify方法调用后，被唤醒的线程**不会立马获得到锁对象**。
- 而是等待notify的synchronized代码块**执行完之后**才会获得锁对象



## sleep和wait有什么区别？

`Thread.sleep()`与`Object.wait()`二者都可以暂停当前线程，释放CPU控制权。

- 主要的区别在于`Object.wait()`在释放CPU同时，**释放了对象锁的控制**。
- 而`Thread.sleep()`没有对锁释放

参考资料：

- [blog.csdn.net/lingzhm/art…](https://link.zhihu.com/?target=https%3A//link.juejin.im/%3Ftarget%3Dhttps%3A%2F%2Fblog.csdn.net%2Flingzhm%2Farticle%2Fdetails%2F44940823)
- [www.cnblogs.com/dolphin0520…](https://link.zhihu.com/?target=https%3A//link.juejin.im/%3Ftarget%3Dhttp%3A%2F%2Fwww.cnblogs.com%2Fdolphin0520%2Fp%2F3920385.html)
- [www.cnblogs.com/eer123/p/78…](https://link.zhihu.com/?target=https%3A//link.juejin.im/%3Ftarget%3Dhttps%3A%2F%2Fwww.cnblogs.com%2Feer123%2Fp%2F7880789.html)
- [www.jianshu.com/p/f4454164c…](https://link.zhihu.com/?target=https%3A//link.juejin.im/%3Ftarget%3Dhttps%3A%2F%2Fwww.jianshu.com%2Fp%2Ff4454164c017)

## 六、finalize()方法

`finalize()`方法将在**垃圾回收器清除对象之前调用**，但该方法不知道何时调用，具有**不定性**

- 一般我们都不会重写它~

> 一个对象的finalize()方法**只会被调用一次**，而且finalize()被调用不意味着gc会立即回收该对象，所以有可能调用finalize()后，该对象又不需要被回收了，然后到了真正要被回收的时候，因为前面调用过一次，所以不会调用finalize()，产生问题。

