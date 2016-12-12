---
title: 继承
type: javaImprove
order: 3
---

> 学习一个新知识的第一步，就是要知道它是什么，然后要知道为什么要用它，最后要知道如何使用它。这篇文章，我们重新认识一下java中的继承。

## **继承是个什么东西**

我们先来看一下上一篇文章中的代码：

> ![这里写图片描述](http://img.blog.csdn.net/20161129160404250)![这里写图片描述](http://img.blog.csdn.net/20161129160417453) 

 你会发现，这两个类中都有name属性，都有marry方法。一个人，不可能只有名字吧。他还有年龄，地址，手机号码，身份证号码，身高，体重巴拉巴拉的。除了男人和女人，还有小孩，老人，教师。。。

如果我们每个类里都写一遍name，age。。。也许你还没写完程序，自己就先累死了。不用我说，大家也应该知道了，没错，我们需要继承的帮助。

我们把相同的属性抽取出来，定义一个新的类Person，然后让男人，女人都去继承它，从而获得Person的属性，这样，就大大简化了我们的工作。

我们来尝试一下。

``` java
//父类
public class Person {
    protected String name;
    protected int age;

    public void eat(){
        System.out.println("i am eating");
    }
}
//Man类 继承Person
public class Man extends Person {
    private boolean hasBeard;

    public void showMan(){
        System.out.println("i am a man");
    }

    public boolean isHasBeard() {
        return hasBeard;
    }
}
//woman类 继承Person
public class Woman extends Person{
    private boolean hasLongHair;

    public void shouWoman(){
        System.out.println("i am a woman");
    }

    public boolean isHasLongHair() {
        return hasLongHair;
    }
}
```
简单的继承我想大家都懂，我就不多说占用篇幅了。
 
## **继承的特点**

我们已经知道了什么是继承，那么继承有没有什么限制呢？

### **1.java中只支持单继承**
也就是说，一个类只能够有一个父类。但是java支持“多重继承”。

单继承：

``` java
class A(){}
class B extends A (){} 
```
多重继承：

``` java
class A{}
class B extends A {} 
class C extends B {} 
```

> 为什么java不支持多继承呢？因为容易造成不必要的混乱。比如说：
>
> - **结构复杂化**：如果是单一继承，一个类的父类是什么，父类的父类是什么，都很明确，因为只有单一的继承关系，然而如果是多重继承的话，一个类有多个父类，这些父类又有自己的父类，那么类之间的关系就很复杂了。
- **优先顺序模糊**：假如我有A，C类同时继承了基类，B类继承了A类，然后D类又同时继承了B和C类，所以D类继承父类的方法的顺序应该是D、B、A、C还是D、B、C、A，或者是其他的顺序，很不明确。
- **功能冲突**：因为多重继承有多个父类，所以当不同的父类中有相同的方法是就会产生冲突。如果B类和C类同时又有相同的方法时，D继承的是哪个方法就不明确了，因为存在两种可能性。

> 当然，多继承的这些问题很多语言已经解决了，比如c++，python等，但并不是所有的语言都有必要去解决这个问题。java的类虽然不能实现多继承，但是java的接口支持多实现，这个我们讲到接口的时候再说。

>对多继承感兴趣的可以google一下mixin（混入），还可以去看一下基于java8的mixin实现（大多数都是线程不安全的，不要随便用）。  

### **2.子类拥有父类非private的属性，方法**
也就是说，父类的属性或者方法如果是peivate的，那么子类是不能继承它的。讲到这里，就必须得提一下四个修饰符了：

| ---- |本类 | 同包（无关类或子类）|不同包（子类）|不同包（无关类）
| --------- |:----:| :-----:|:--:|:-:
| private | ✅ | |
| default | ✅ | ✅ |
| protected| ✅ | ✅|✅
|public|✅|✅|✅|✅

在java中，protected关键字大展身手的地方就是在继承中。《thinking in java》中是这样介绍protected的：

> 在理想世界中，仅靠关键字private已经足够了。但在实际项目中，经常会想要将某些事物尽可能堆这个世界隐藏起来，但仍然允许导出的类的成员访问他们。关键字protected就是起这个作用的。它指明”就类用户而言，这是privated，但是对于任何一个继承于此类的导出类或其他任何一个位于同一个包内的类来说，他却是可以访问的”

怎么理解呢？写个代码你就明白了

``` java

package cn.pkgA
class A {
	protected String name；
}
class B extends A{}
class C {
	B b = new B();
	b.name;//可以访问到
}
package cn.pkgB

class C {
	B b = new B();
	b.name;//访问不到
}


```

### **3.子类可以拥有自己的属性和方法，即子类可以对父类进行扩展。**

如果子类只能有父类的属性和方法，那要子类还有什么用？？

### **4.子类可以用自己的方式实现父类的方法。**

这个叫做函数重写（覆盖），我们一会会重点分析。


## **构造器**

除了被peivate修饰的方法和变量之外，父类的构造器也不能被子类继承。

但是父类的构造器带有参数的，则必须在子类的构造器中显式地通过super关键字调用父类的构造器并配以适当的当属列表。

如果父类有无参构造器，则在子类的构造器中用super调用父类构造器不是必须的，如果没有使用super关键字，系统会自动调用父类的无参构造器。

我们给Person类添加一个构造器：

``` java
public class Person {
    protected String name;
    protected int age;

    public void eat(){
        System.out.println("i am eating");
    }
	//带参数的构造器
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

这个时候，如果你不给子类添加构造器并在第一行写入super（name，age），则会报错：

> ![这里写图片描述](http://img.blog.csdn.net/20161129165450586) 

在子类添加如下方法后错误消失：

``` java
public Man(String name, int age/*, boolean hasBeard*/) {
    super(name, age);
    //this.hasBeard = hasBeard;
}

//注释掉的地方可有可无
```
为什么会有这个要求呢？你一会就知道了，先卖个关子。


## **重写与重载**

### **重写**
重写又叫覆盖，发生在继承关系下的子类中。我们上面说过，子类可以用自己的方式实现父类的方法，重写不能改变参数列表，也不能缩小方法的访问权限，如果父类方法抛出异常，子类抛出的异常不能比父类的异常“大”，也不能抛出新的异常。

我们Person类中有一个方法：

``` java
    public void eat(){
        System.out.println("i am eating");
    }
```
有一个子类修道成仙了，不吃饭，于是他可以在他自己的类里这样改
``` java
	@Override  //这个是注解，表明这个方法是重写了父类的方法，最好写上
    public void eat(){
        System.out.println("i don't eat");
    }
```

这里提一下，子类重写父类方法不能缩小父类方法的访问权限但扩大是可以的。比如说父类有一个protected方法，子类重写它的时候不能改为private，但是可以改成public。

这里还有一个不大不小的坑。如果你的父类方法是peivate的，比如：

``` java
    private void eat(){
        System.out.println("i am eating");
    }
```

你可以在子类中这样写：

``` java
    public void eat(){
        System.out.println("i don't eat");
    }
```
但是，这不是重写！！！！因为父类方法是私有的，所以子类根本没有得到eat()这个方法，子类的eat()方法是你重新定义的一个和父类没有半毛钱的函数。

### **重载**
把重载放到这里讲只是因为它和重写有的然傻傻分不清楚，重载和继承没有任何关系（当然，继承之间也存在重载，也就是说，继承可以重载，但是重载不一定继承），它发生在类本身。重载方法的特点是方法名相同而参数列表不同。

比如这样：

``` java
public void count(int a , int b){
	System.out.println("a+b=" + (a+b));
}

public void count(int a , int b,int c){
	System.out.println("a+b=" + (a+b+c));
}

public void count(int a , int b ,double c){
	System.out.println("a+b=" + (a+b+c));
}

```
函数重载的特点：

 - 被重载的方法**必须**改变参数列表(参数个数或类型或顺序不一样)；
 - 被重载的方法可以改变返回类型；
 - 被重载的方法可以改变访问修饰符；
 - 被重载的方法可以声明新的或更广的检查异常（区别于重写）；
 - 方法能够在同一个类中或者在一个子类中被重载。

注意：参数列表必须不同！


## **继承的缺点**

 - 继承是一种强耦合关系，父类变，子类就必须变。
 - 继承破坏了封装，对于父类而言，它的实现细节对与子类来说都是透明的。

提醒！慎用继承！

如果你知道高手写代码都想着怎么解耦你就知道这个缺点室友多么讨厌了。

你可能会问，我不用继承用什么？别急，接下来的几篇文章会告诉你。

## **昨天的遗留问题**

看了上一篇文章的人可能还记得那个遗留问题。我们现在来解决一下：

``` java
//父类
public class Person {
    protected String name;
    
    public void marry(Person p){
        System.out.println("marry");
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
//man类
public class Man extends Person {
    private Woman wife;
    private double money;

    @Override
    public void marry(Person p) {
        this.wife = (Woman)p;
        p.marry(this);
    }
	//只有自己和妻子可以用钱
    public void setMoney(Person p,double money) {
        if (p == this || p == this.wife)
            this.money = money;
        else
            System.out.println(p.getName()+"抢钱！");
    }

    public double getMoney() {
        return money;
    }
}
//woman类
public class Woman extends Person{
    private boolean hasLongHair;
    private Man husband;

    @Override
    public void marry(Person p) {
        this.husband = (Man)p;
    }
}
```

我们来看一下效果：

> ![这里写图片描述](http://img.blog.csdn.net/20161129215239122) 

>看起来还不错，不是么。

当然，我更喜欢这么做

``` java
public void setMoney(Person p,double money) {
	  if (p == this || p == this.wife)
	      this.money = money;
	  else if(money > this.money)
		  this.money = money;
	  else  
	      System.out.println(p.getName()+"抢钱！");
}
```
 

## **总结**

继承还有很多知识点，比如向上转型和向下转型（上面解决上一篇问题的代码就用到了这个知识点），在继承中，对象是怎么初始化的，静态代码块的使用，final关键字的使用等等。

但是我打算先放一放再讲，等写完组合，聚合和多态再来讨论这些知识会更好一点。

下一篇《重新认识java（四） ---  组合、聚合与继承的爱恨情仇》敬请期待。


----------
有错误或者我没讲到的地方或者更好的思路请及时与我联系！