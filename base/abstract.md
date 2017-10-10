# 抽象

#### 泛型中extend 和 super的区别

https://itimetraveler.github.io/2016/12/27/%E3%80%90Java%E3%80%91%E6%B3%9B%E5%9E%8B%E4%B8%AD%20extends%20%E5%92%8C%20super%20%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F/

<? extends T> 是指 “上界通配符”（Upper Bounds Wildcards）

<? super T> "下界通配符"（Lower Bounds Wildcards）

上界<? extends T> 不能往里存，只能往外取，但是读取出来的东西只能存放在T里面或者他的基类里面。

下界<? super T> 不影响往里存，但是往外取只能放在Object对象里。

**PECS原则**（Producer extends Consumer super）

​	频繁往外读取内容，适合上界Extends

​	经常往里插入的，适合用下界Super




#### 抽象类和接口的区别

1. 默认的方法实现 抽象类可以有默认的方法实现完全是抽象的。接口根本不存在方法的实现
2. 实现 子类使用extends关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。
   子类使用关键字implements来实现接口。它需要提供接口中所有声明的方法的实现
3. 构造器
   抽象类可以有构造器
   接口不能有构造器
4. 与正常Java类的区别
   除了你不能实例化抽象类之外，它和普通Java类没有任何区 接口是完全不同的类型
5. 访问修饰符
   抽象方法可以有public、protected和default这些修饰符 接口方法默认修饰符是public。你不可以使用其它修饰符。
6. main方法
   抽象方法可以有main方法并且我们可以运行它
   接口没有main方法，因此我们不能运行它。
7. 多继承
   抽象类在java语言中所表示的是一种继承关系，一个子类只能存在一个父类，但是可以存在多个接口。
8. 速度
   它比接口速度要快
   接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法。
9. 添加新方法
   如果你往抽象类中添加新的方法，你可以给它提供默认的实现。因此你不需要改变你现在的代码。
   如果你往接口中添加方法，那么你必须改变实现该接口的类。