---
title: 万物皆对象(下)
type: javaImprove
order: 2
---

## **对象的强引用，软引用，弱引用和虚引用**
  
  Java中是JVM负责内存的分配和回收，这样虽然使用方便，程序不用再像使用c那样操心内存，但同时也是它的缺点(不够灵活)。为了解决内存操作不灵活这个问题，可以采用软引用等方法。

先介绍一下这四种引用：

- 强引用

 > 以前我们使用的大部分引用实际上都是强引用，这是使用最普遍的引用。如果一个对象具有强引用，那就类似于必不可少的生活用品，垃圾回收器绝不会回收它。当内存空 间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。


- 软引用（SoftReference）

  > 如果一个对象只具有软引用，那就类似于可有可物的生活用品。如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。
   
   > 软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA虚拟机就会把这个软引用加入到与之关联的引用队列中。

 
- 弱引用（WeakReference）
    > 如果一个对象只具有弱引用，那就类似于可有可物的生活用品。弱引用与软引用的区别在于：只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它 所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程， 因此不一定会很快发现那些只具有弱引用的对象。 
   
   >  弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。


- 虚引用（PhantomReference）
    > "虚引用"顾名思义，就是形同虚设，与其他几种引用都不同，虚引用并不会决定对象的生命周期。如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。
    
    > 虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与软引用和弱引用的一个区别在于：虚引用必须和引用队列（ReferenceQueue）联合使用。当垃 圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是 否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

在实际开发中，弱引用和虚引用不常用，用得比较多的是软引用，因为它可以加速jvm的回收。

软引用的使用方式：

![这里写图片描述](http://img.blog.csdn.net/20161127203117708) 

关于软引用，我之后会单独写一篇文章，所以这里先一笔带过。

## **对象的复制**

java除了用new来创建对象，还可以通过clone来复制对象。

那么这两种方式有什么相同和不同呢？ 

- new

>new操作符的本意是分配内存。程序执行到new操作符时，首先去看new操作符后面的类型，因为知道了类型，才能知道要分配多大的内存空间。分配完内存之后，再调用构造函数，填充对象的各个域，这一步叫做对象的初始化，构造方法返回后，一个对象创建完毕，可以把他的引用（地址）发布到外部，在外部就可以使用这个引用操纵这个对象。

- clone
>clone在第一步是和new相似的， 都是分配内存，调用clone方法时，分配的内存和源对象（即调用clone方法的对象）相同，然后再使用原对象中对应的各个域，填充新对象的域， 填充完成之后，clone方法返回，一个新的相同的对象被创建，同样可以把这个新对象的引用发布到外部。


如何利用clone的方式来得到一个对象呢？

看代码：

>![这里写图片描述](http://img.blog.csdn.net/20161127204300965)

>对Person类做了一些修改

看实现代码：

> ![这里写图片描述](http://img.blog.csdn.net/20161127204359354)

这样就得到了一个和原来一样的新对象。


## **深复制和浅复制**

但是，细心并且善于思考的人可能一经发现了一个问题。

age是一个基本数据类型，支架clone没什么问题，但是name可是一个String类型的啊。我们clone后的对象里的name和原来对象的name是不是指向同一个字符串常量呢？

做个试验：

> ![这里写图片描述](http://img.blog.csdn.net/20161127205753811) 

> 果然，是同一个对象。如果你不能理解，那么看这个图。

> ![这里写图片描述](http://img.blog.csdn.net/20161127210126755)

> 其实如果只是String还好，因为String的不可变性，当你随便修改一个值的时候，他们就会指向不同的地址了，但是除了String，其他都是可变的。这就危险了。


上面的这种情况，就是浅克隆。这种方式在你的属性列表中有其他对象的引用的时候其实是很危险的。所以，我们需要深克隆。也就是说我们需要将这个对象里的对象也clone一份。怎么做呢？

在内存中通过字节流的拷贝是比较容易实现的。把母对象写入到一个字节流中，再从字节流中将其读出来，这样就可以创建一个新的对象了，并且该新对象与母对象之间并不存在引用共享的问题，真正实现对象的深拷贝。
```
//使用该工具类的对象必须要实现 Serializable 接口，否则是没有办法实现克隆的。
public class CloneUtils {

    public static <T extends Serializable> T clone(T   obj){
        T cloneObj = null;
        try {
            //写入字节流
            ByteArrayOutputStream out = new ByteArrayOutputStream();
            ObjectOutputStream obs = new   ObjectOutputStream(out);
            obs.writeObject(obj);
            obs.close();

            //分配内存，写入原始对象，生成新对象
            ByteArrayInputStream ios = new  ByteArrayInputStream(out.toByteArray());
            ObjectInputStream ois = new ObjectInputStream(ios);
            //返回生成的新对象
            cloneObj = (T) ois.readObject();
            ois.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
        return cloneObj;
    }
}
```
使用该工具类的对象只要实现 Serializable 接口就可实现对象的克隆，无须继承 Cloneable 接口实现 clone() 方法。

测试一下：

> ![这里写图片描述](http://img.blog.csdn.net/20161127211824152)

> 很完美

>这个时候，Person类实现了Serializable接口

是否使用复制，深复制还是浅复制看情况来使用。


> 关于序列化与反序列化以后会讲。


----------
这篇文章到这里就暂时告一段落了，后续有补充的话我会继续补充，有错误的话，我也会及时改正。欢迎大家提出问题。

> 事例代码放在github：https://github.com/CleverFan/JavaImprove