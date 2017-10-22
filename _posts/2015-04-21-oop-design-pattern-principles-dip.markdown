---
layout: single
title: "OOP设计模式原则之依赖倒置原则"
categories: Technology OOP
description: 高层模块不应该依赖底层模块，两者都应该依赖其抽象
date: 2015-04-21T08:46:18+00:00
---

依赖倒置原则（Dependence Inversion Principle，DIP）的原始定义：高层模块不应该依赖底层模块，两者都应该依赖其抽象；抽象不应该依赖细节；细节应该依赖抽象。

依赖倒置原则在Java语言中的表现是：

- 模块间的依赖通过抽象发生，实现类之间不发生直接的依赖关系，其依赖关系是通过接口或者抽象类产生的；
- 接口或抽象类不依赖于实现类；
- 实现类依赖接口或抽象类。

依赖倒置原则实际上就是要求**“面向接口编程”**。

**采用依赖倒置原则可以减少类间的耦合性，提高系统的稳定性，降低并行开发引起的风险，提高代码的可读性和可维护性。**

例如：

~~~java
// 司机接口
public interface IDriver {
    public void driver(ICar car);
}

// 司机实现类
public class Driver implements IDriver {
    public void driver(ICar car) {
        car.run();
    }
}

// 汽车接口
public interface ICar {
    public void run();
}

public class Benz implements ICar {
    public void run() {
      System.out.println("奔驰汽车开始运行...");
    }
}
public class BMW implements ICar {
    public void run() {
      System.out.println("宝马汽车开始运行...");
    }
}

//场景类
public class Client {
    public static void main(String[] args) {
        IDriver zhangSan = new Driver();
        ICar benz = new Benz();
        zhangSan.driver(benz);
    }
}
~~~

![](http://7xil47.com1.z0.glb.clouddn.com/ood_dip.png)

抽象是对实现的约束，对依赖者而言，也是一种契约，不仅仅约束自己，还同时约束自己与外部的关系，其目的是保证所有的细节不脱离契约的范畴，确保约束双方按照既定的契约（抽象）共同发展，只要抽象这根基线在，细节就脱离不了这个圈圈，始终让你的对象做到“言必信，行必果”。

**依赖的三种写法**

只要做到抽象依赖，即使是多层的依赖传递也无所谓惧！

A. 构造函数传递依赖对象——构造函数注入

~~~java
public class Driver implements IDriver {
    private ICar car;
    public Driver(ICar _car) {
        this.car = _car;
    }
    public void driver() {
        this.car.run();
    }
}
~~~

B. Setter方法传递依赖对象——Setter依赖注入

~~~java
public class Driver implements IDriver {
    private ICar car;
    public void setCar(ICar car) {
        this.car = car;
    }
    public void driver() {
        this.car.run();
    }
}
~~~

C. 接口声明依赖对象——接口注入

~~~java
public class Driver implements IDriver {
    public void driver(ICar car) {
        car.run();
    }
}
~~~

**本质：**

依赖倒置原则的本质就是通过抽象（接口或者抽象类）使各个类或模型的实现彼此独立，不互相影响，实现模块间的松耦合。

**规则：**

- 每个类尽量都有接口或抽象类，或者抽象类和接口两者都具备；
- 变量的表面类型尽量是接口或者抽象类；
- 任何类都不应该从具体类派生；
- 尽量不要覆写基类的方法；
- 结合里氏替换原则使用。

接口负责定义public属性和方法，并且声明与其他对象的依赖关系，抽象类负责公共构造部分的实现，实现类准确的实现业务逻辑，同时在适当的时候对父类进行细化。

**依赖倒置与依赖正置**

依赖正置就是类间的依赖是实实在在的实现类间的依赖，也就是面向实现编程，这也是正常人的思维方式，我要开奔驰车就依赖奔驰车，我要使用笔记本电脑就直接依赖笔记本电脑，而编写程序需要的是对现实世界的事物进行抽象，抽象的结构就是有了抽象类和接口，然后我们根据系统设计的需要产生了抽象间的依赖，代替了人们传统思维中的事物间的依赖，“倒置”就是从这里产生的。
