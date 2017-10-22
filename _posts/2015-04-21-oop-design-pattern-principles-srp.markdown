---
layout: single
title: "OOP设计模式原则之单一职责原则SRP "
categories: Technology OOP
description: 就一个类而言，应该仅有一个引起它变化的原因。
date: 2015-04-21T08:05:16+00:00
---

**定义：就一个类而言，应该仅有一个引起它变化的原因。**

变化是指具体类的改变，如一个modem类具有连接管理和数据通讯的功能，那么这个类就有连接管理和数据通讯这两个变化方向，此时就违背了“单一职责的原则”。

**关于“单一职责”**

“单一职责”也就是“单一变化原因”。“职责”也就是引起类变化的原因。

“单一职责原则”是面向对象设计的第一个基本原则，它可能是最简单的也可能是最难运用的一个原则！

通常，一个类的“职责”越多，导致其变化的因素也就越多。因为每个职责都可能是一个变化的轴线。这与我们在设计类时所采用的方法有关系。一般情况下，我们在设计一个类的时候会把与该类有关操作都组合到这个类中，这样做的后果就有可能将多个职责“耦合”到了一块，当这个类的某个职责发生变化时，很难避免类的其它部分不受影响，这最终导致程序的“脆弱”和“僵硬”。

解决这种问题的方法就是“分耦”，将不同的职责分别进行封装，不要将其组合在一个类中，使这个类只有一个可能会引起它变化的原因。这样做将会使你的程序易于修改和维护。但这个过程可能是很困难的，因为我们不总是能轻易知道那些职责会发生变化，那些职责应该被提取出来。所以，你的程序可能会有一个演化的过程，从中得出这些会发生的职责并进行另外的封装。需要注意的一点就是当实标情况中的职责确实发生了变化，应用该原则才是有意义的。如果一个类组合了多个职责，但这些职责在实际情况中根本不会发生变化，那完全没有必要提前费尽心机去应用这个原则。

**总结**

SRP好处：消除耦合，减小因需求变化引起代码僵化性臭味

注意点：

1. 一个合理的类，应该仅有一个引起它变化的原因，即单一职责；
2. 在没有变化征兆的情况下应用SRP或其他原则是不明智的；
3. 在需求实际发生变化时就应该应用SRP等原则来重构代码；
4. 使用测试驱动开发会迫使我们在设计出现臭味之前分离不合理代码；
4. 如果测试不能迫使职责分离，僵化性和脆弱性的臭味会变得很强烈，那就应该用Facade或Proxy模式对代码重构；

**实例**

违反SRP原则代码: modem接口明显具有两个职责：连接管理和数据通讯；

~~~php
class Modem
{
    public void dial(string pno);
    public void hangup();
    public void send(char c);
    public void recv();
}
~~~

此时应该把的modem的两个职责分到两个类中

~~~php
class DataChannel
{
    public void send(char c);
    public void recv();
}
class Connection
{
    public void dial(string pno);
    public void hangup();
}
~~~

新的modem类：

~~~php
class modem
{
    //构造
    public modem()
    {
        datachannel = new DataChannel();
        con = new Connection();
    }
    //字段
    private DataChannel  datachannel;
    private  Connection  con;
    //操作
    public void dial(string pno)
    {
        datachannel.dial();
    }
    public void hangup()
    //  .......
    public void send(char c)
    //  .......
    public void recv()
    //  .......
}
~~~

![](http://7xil47.com1.z0.glb.clouddn.com/ood_srp.png)
