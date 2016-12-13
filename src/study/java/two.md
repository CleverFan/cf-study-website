---
title: 封装
type: javaImprove
order: 3
---

> 如果你认为封装仅仅是private + getter and setter，那你就大错特错了！

## **什么是封装**

对于面向对象的特点，我想大家都可以倒背如流了：封装，继承，多态。很多人对这些特点的理解仅仅停留在表面。以为封装就是变量私有化，然后对外提供接口，而不知为什么要这样做。

封装，简单的来讲就是将变量的属性私有化，在java里就是用private修饰符修饰，这样在外部产生的对象就不能直接访问到这个变量。想要对变量进行操作或者访问，就需要在类里提供外部访问的接口，也就是我们熟知的get和set方法。

这就是大部分人对封装的理解。知道有封装这回事，知道怎么用，却不知道为什么要用，甚至觉得这是多此一举。因为明明`person.name`就可以访问到变量，为什么非要`person.getName()`呢？

## **任性的使用public**

我们先来看一下不使用封装的情况。

首先，有两个类，Man和Women:

``` java
//Man
public class Man {
    public String name; //名字
    public Woman wife;  //男人嘛，都有妻子
    public double money;//男人嘛，多赚点钱

	//还可以结个婚
    public void marry(Woman woman){
        this.wife = woman;
        woman.marry(this);
    }
}
//Women
public class Woman {
    public String name; //名字
    public Man husband; //得有一个丈夫
	//也可以结个婚
    public void marry(Man man){
        this.husband = man;
    }
}
```
代码很精简，看着很舒服，测试一下。

> ![这里写图片描述](http://img.blog.csdn.net/20161128181621627) 

> 哎哟，看起来还不错。

这个时候，来了一个小偷，这小偷不干别的，就偷别人的钱和老婆。

``` java
//小偷
public class Thief {
    private double stealMoney = 0;
    private List<Woman> womens = new ArrayList<>();

    //偷钱
    public void stealMoney(Man man){
        stealMoney += man.money;
        man.money = 0;
        System.out.println("哈哈，偷到钱了");
    }
    //偷老婆，最可气的是，偷了你的老婆还把凤姐丢给了你
    public void stealWife(Man man){
        womens.add(man.wife);
        Woman woman = new Woman();
        woman.name = "凤姐";
        man.wife = woman;
        System.out.println("哈哈哈，又偷了一个妹纸");
    }
}
```

有一天，来了这么一个小偷：

> ![这里写图片描述](http://img.blog.csdn.net/20161128182205942)

>傻了吧？你老婆呢？你钱呢？哈哈哈哈哈哈哈哈哈

就这样，小偷偷走了你的钱和你的老婆并丢给了你一个凤姐，而你，却无能为力。

你觉得必须要改变一下了！！

## **封装前来报到** 

封装觉得你有点惨，于是过来帮了一下你：

``` java
//PackageMan
public class PackageMan {
    private String name; //私有化名字
    private PackageWoman wife;//必须私有！！必须！
    private double money; //私有，统统私有！
    //我们先写个构造函数，为了方便
    public PackageMan(String name, double money) {
        this.name = name;
        this.money = money;
    }
    //结婚
    public void marry(PackageWoman woman){
        this.wife = woman;
        woman.marry(this);
    }

    //各种getter和setter
    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public PackageWoman getWife() {
        return wife;
    }

    public double getMoney() {
        return money;
    }
}

//PackageWoman
public class PackageWoman {
    private String name;
    private PackageMan husband;

    public void marry(PackageMan man){
        this.husband = man;
    }

    public PackageWoman(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public PackageMan getHusband() {
        return husband;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```
看起来有点眼花缭乱，这样真的有效么？

> ![这里写图片描述](http://img.blog.csdn.net/20161128183404652)

> 恩，看起来还行，就是代码长了点，赶紧给我来个小偷测试一下！

什么？你想测试一下，不好意思，测试不了，因为小偷已经死在测试的路上了。因为我们根本没有对外提供设置money和wife的方法，所以小偷可以知道你有多少钱，知道你有一个漂亮的老婆，但是他却无能为力，因为他只能看着。

细心的人也许发现了，这里面有一个很严重的问题：

没错，小偷不能把我们的money清空了，也不能将我们的wife换成别人了。但是，如果我们要自己换呢？我的钱这辈子就这么点？还不能花？我还不能离婚了？（咳咳。。不鼓励离婚哈，就是举个例子，别打我）

## **这才是封装厉害的地方**

如何解决上面的问题呢？私有化外部访问不到，自己也没法改变数据，提供set方法又会让所有人都能改，和不私有没什么区别，好纠结啊。


等等，你刚刚说 “所有人“？真的是所有人么？

我们来看看：

``` java
public void setMoney(PackageMan man,double money) {
    if (man == this) {
        this.money = money;
    } else {
        System.out.println("喂，110吗？"+man.getName()+"抢钱！");
    }
}
```

这样呢？只有你自己可以修改，别人都不可以，测试一下：

> ![这里写图片描述](http://img.blog.csdn.net/20161128184912423) 

> 这样就可以了，自己可以修改，但是别人不可以。

但是你老婆不满意了，凭什么只有你自己能改？我也想改！

这种要求，还是应该满足一下的，怎么做呢？

``` java
public void setMoney(Object o,double money) {
    if (o == this || o == this.wife) {
        this.money = money;
    } else {
        System.out.println("喂，110吗？有人抢钱！");
    }
}
```

这样就可以了。

当然，爱思考的人肯定发现了，我把抢钱的那句话修改了，没有获取修改人的名字，因为传入的是Objects对象。当然，你也可以再写点代码，判断一下传进来的是什么，然后抢转为相应的类型，再调用相应的方法。

除此之外，就没有别的办法了么？当然有，具体怎么做，我们下一篇文章再做分解。下一篇文章《重新认识java（三） ---- 面向对象之继承》不定期更新。

> 仔细思考一下你会发现，我特么竟然是在骗你！因为当你提供了set函数以后，小偷又可以偷你的东西了。仔细看一下之前小偷是怎么偷你东西的你就知道了。

> 没错，就是通过你自己。小偷通过你自己改变了你自己。听起来有点扯，但是事实上就是这样的。

> 那么，有没有一种办法让小偷在只得到”你自己“的情况下怎么样都不能改变“你的属性值”，而只有你自己能改变呢？

> 大家可以自己想想，具体的解决办法，我们在之后的文章里揭晓。敬请期待。

## **总结一下**

以上就是面向对象的封装，封装不仅仅只是private + getter and setter。使用封装可以对setter进行更深层次的定制。你可以对可以执行setter方法的对象做规定，也可以对数据作要求，还可以做类型转换等等一系列你可以想到的。

使用封装不仅仅是安全，更可以简化操作。不要觉得用了封装多了好多代码，看起来乱糟糟的。这只是因为我举得例子太小了。如果你写你个大的系统，一开始你的这样定义类的

``` java
public int age;
```

你的程序里大概有100处这样的语句：

``` java
p.age = 10;
```

这个时候，突然要求你把数据类型变了，改成：

``` java
public String age;
```
你是不是要把那100处数据都加个双引号呢？这是不是很麻烦？

如果你用了封装，你只需要这样：

``` java
public void setAge(int age){
	this.age = String.valueOf(age);
}

```
然后就搞定了，是不是简化了操作？

我只是举个例子，实际开发中也不会出现改变数据类型这么操蛋的事。。


封装还有一个好处是模块化。当你参与一个很多人实现的大型系统中，你不可能知道所有的类是怎样实现的。你只需要知道这个类给我提供了哪些方法，我需要传入什么数据，我能得到什么结果。至于怎么得到的，关我x事？

所以说，如果你写的代码还没有用封装，改过来吧。不是因为大家都用所以你也应该用，而是这确实可以给你提供极大的便利。

结束~

----------
本文同步更新在 ： blog.clevercfan.cn
有什么疑问或者错误可以给我留言

下篇文章见~