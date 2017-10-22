---
layout: single
title: "OOP设计模式原则之开放—封闭原则"
categories: Technology OOP
description: 指软件实体（类，模块，函数等等）应该可以扩展，但是不可以修改。即对于扩展开放，而对于修改封闭。
date: 2015-04-21T08:06:48+00:00
---

首先，让我们分析一下背景。什么是软件开发过程中最不稳定的因素？——答案是需求！需求在软件开发过程中时时刻刻都可能发生变化。那么，如何灵活应对变化是软件结构设计中最重要也是最困难的一个问题。好的设计带来了极大了灵活性，不好的设计则充斥着僵化的臭味。这样，也就引出了本文的主题：【开发封闭原则】。

下面，就来简单扼要的介绍一下什么是【开放封闭原则】。

**【开放封闭原则】包括两个特征：对于扩展是开放的；对于修改是封闭的。**

对于扩展开放，意味着模块的行为是可扩展的。对于修改封闭，就是说在扩展模块行为的同时不对任何既有代码或二进制代码（.jar）进行修改。当然，这里说的过于绝对。有时为了完成某些任务不得不改动既有代码。但是，我们的目标是尽量的遵守原则。

**那如何才能做到对扩展开发，对修改封闭呢？关键在于抽象！那什么是抽象呢？**

我个人的理解是：抽象是对事物本质的概念上的理解。面向对象的分析中应该以行为分析为主线。举一个例子说明：假如从北京到上海，我们可以坐飞机，可以坐火车，可以坐汽车，也可以骑自行车。那对于这个问题如何分析呢？这个问题的抽象又是什么呢？也许可以这样思考：飞机、火车、汽车和自行车在本质上都是交通工具。所以得到这样的分析结果：交通工具被抽象为父类，飞机、火车、汽车和自行车都是其特例化的实现，父类中定义一个抽象方法，可以将人从一个地方运送到另外一个地方，各个子类重新定义运送的方式。这个设计无可厚非！但是，注意了！如果哪天某个人心情不错，想从北京走到上海了。那他的交通工具又是什么呢？11路！如果把步行也算成交通工具那就太不符合实际了。所以，面向对象的分析角度，不应该是从事物物理方面的关联去分析，而应该是从事物的行为方面的管理区分析。对于这个问题，一个人从北京道上海，无论是怎么到达的，只不过是移动策略不同。

下面，就给出相应的示例代码来说明一下上述问题。

~~~java
class Traveller {
    private Car car = new Car();
    public void travel(Address srcAddress,Address destAddress){
        car.move(srcAddress,destAddress);
    }
}
~~~

这是最开始直接使用Car的旅行者，如果想替换成AirPlane怎么办？修改代码，用new AirPlane()代替Car。**面向对象的实践原则指出：面向接口编程，而不面向实现编程。**当代码依赖于具体实现时，就缺失了灵活性，面对新的扩展（也就是新的实现），必须修改既有代码。

接着，给出设计灵活的代码：

~~~java
class Traveller {
    private TravelStrategy _strategy;
    public void travel(Address srcAddress,Address destAddress){
        _strategy.move(srcAddress,destAddress);
    }
    public void setTravelStrategy(TravelStrategy strategy){
        _strategy = strategy;
    }
}

public interface TravelStrategy{
    void move(Address srcAddress,Address destAddress);
}

public class CarStrategy implements TravelStrategy{
    public void move(Address srcAddress,Address destAddress){
    //    ...
    }
}

public class AirPlaneStrategy implements TravelStrategy{
    public void move(Address srcAddress,Address destAddress){
    //    ...
    }
}

public class WalkStrategy implements TravelStrategy{
    public void move(Address srcAddress,Address destAddress){
    //    ...
    }
}
~~~

![](http://7xil47.com1.z0.glb.clouddn.com/ood_ocp.png)

使用这种设计就能灵活的应对设计。比如说：现在需要另外一种从北京到上海的方式——爬。呵呵，也许这种行为不可思议，但它也能达到目的。那如何扩展呢？只需要扩展一个新的TravelStrategy实现即可。这样，Traveller类不需要修改任何代码！可以通过调用setter方法来切换移动策略。现在还有一个问题：如何确定实例化哪个策略？这里可以引用工厂，专门负责对象实例化。可以将需要使用的策略放在文件中，这样改变策略时就不需要修改源代码了。但当新增策略时，还是需要修改Factory。没办法，现实中没有完全符合原则的情况，我们只能尽力去遵守！

现在，我们看一下以上代码中类之间的关系。Traveller类、TravelStrategy接口及其实现类，Traveller类是TravelStrategy接口的客户代码。那Traveller和TravelStrategy的关系与TravelStrategy和它的实现类之间的关系哪个更紧密一些呢？答案是前者。这也正是另外一个敏捷原则**【依赖倒置原则】**。高层代码不应该依赖低层代码，低层代码要依赖于高层代码。在这里，说白了的意思就是：一个旅行者要从北京到上海，而旅行团已经给他安排好了去的方法，他本身并不关心怎么到达，只需要知道有办法到达就可以了。这里旅行者和到达方法之间的关系就非常密切了，而具体如何到达那是低层次的问题了。

上述的问题与实现是实现【开放封闭原则】的一种常用方法——**策略模式**。还有一种常用的实现方法：**模板方法**。其实，对于这两种方法的本质所在也就是面向对象中的组合与继承。

我们已经学会了如何封装变化，那何时才封装呢？掌握了这项技能是一件好事，但滥用就出问题了~！因为遵循OCP的代价是昂贵的，创建正确的抽象是要花费时间和精力的，同时那些抽象也增加了软件设计的复杂性。通常，我们采用这种办法：**只受愚弄一次**。也就是说，最初的实现不封装任何东西。当真正的变化到来了，重构代码，封装变化，以避免同类问题再次发生。

OCP是面向对象设计的核心所在。开发人员应该仅仅对程序中呈现出频繁变化的那些部分做出抽象。拒绝不成熟的抽象与抽象本身一样重要。

最后，在简单介绍一下面向对象分析的常用方法：

- 寻找问题域中的各个事物或行为共性
- 从共性中创建抽象
- 从共性的变化中创建派生
- 看共性之间的关系如何
