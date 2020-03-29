操作符重载使用什么方法。

![image-20200118234802793](操作符重载Operator Overloading.assets/image-20200118234802793.png)

模板Class Templates 

### 模板类

![image-20200118235134650](操作符重载Operator Overloading.assets/image-20200118235134650.png)

### 函数模板：

![image-20200118235305822](操作符重载Operator Overloading.assets/image-20200118235305822.png)

![image-20200118235337316](操作符重载Operator Overloading.assets/image-20200118235337316.png)

![image-20200118235343819](操作符重载Operator Overloading.assets/image-20200118235343819.png)

编译器会做是实际参数的推导，因此在比较大小的时候，T为stone那么就会去调用stone::operator<()

操作。	

### 成员模板

![image-20200119000531617](操作符重载Operator Overloading.assets/image-20200119000531617.png)

## Specialization 特化，

侯捷老师拒了个例子，关于计算机图形学中绘制一条直线的案例。

我们学校过一条直线的数学方程式，两个点之间的连线上的所有点，是实数。

但是计算机屏幕上实际上是一个又一个的像素点，在表示成坐标的时候都是整数。

因此某位大佬就提出了同一种特例算法，这些算法引用到绘制图形的时候非常快。![image-20200119002038726](操作符重载Operator Overloading.assets/image-20200119002038726.png)

比如：下面这行调用，匹配泛华，不能匹配特例，因此直接使用泛华方法。

![image-20200119002213196](操作符重载Operator Overloading.assets/image-20200119002213196.png)

这是hash散列表的一部分

![image-20200119002736547](操作符重载Operator Overloading.assets/image-20200119002736547.png)

Partial Specialization偏特化

![image-20200119003028835](操作符重载Operator Overloading.assets/image-20200119003028835.png)·

如果T是bool类型，那么可以使用更加精简的方式对其进行存储。对这种类型进行特例化、

![image-20200119003758555](操作符重载Operator Overloading.assets/image-20200119003758555.png)

泛化传进来的参数如果是一个指针，在进行偏偏特化、