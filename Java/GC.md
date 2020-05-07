# GC

GC目的：回收堆内存中不再使用的对象，释放资源

回收时间：当对象永久地失去引用后，系统会在合适的时候回收它所占的内存

java中使用:

```java
System.GC(); // or Runtime.getRuntime().gc() 两者一致
```

## 分区

Sun的JVM将整个堆分为三代：

- Young Gen(新生代)
- Old Gen(老年代)
- Perm Gen(持久区)

GC类型：

- Minor GC:通常是指对新生代的回收
- Major GC:通常是指对老年代的回收
- Full GC: Major GC除并发GC外均需对整个堆进行扫描和回收

算法：

- 复制拷贝算法：要拷贝大量数据，不会产生碎片
- 标记算法：从引用根节点开始标记所有被引用对象，把未被引用的对象清楚。要遍历所有对象，会产生碎片。

### young gen

young gen 又分为：

- Eden
- survivor1(from space)
- survivor2(to space)

young gen里面的对象生命周期比较短，GC对这些对象进行回收的时候采用的是复制拷贝算法。

Eden 每当一个对象创建的时候都会分配这个区域，当无法分配的时候，就会触发一次Minor GC。GC每次回收的时候都将Eden区存活的对象和survivor1中的对象拷贝到survivor2中，然后把Eden和survivor1清空；当GC执行下次回收的时候将Eden和survivor2中的对象拷贝到survivor1中，清空Eden和survivor2。依次这样执行；经过数次回收将依然存活的对象复制到Old Gen区。

### old gen

当对象从年轻代晋升到老年代之前，会检测老年区的剩余空间是否大于要晋升对象的大小，如果小于则直接执行一次full GC，以便让老年区腾出更多的空间，然后再进行Minor GC，把年轻代的对象复制到老年代；如果大于，则根据条件(HandlePromotionFailure设置)进行Minor GC和Full GC。

老年区采用标记算法，因为老年区对象的生命周期都是比较长的，采用拷贝算法要拷贝大量的数据。采用标记算法，每次GC回收都要遍历所有的对象。

### perm gen

Perm Gen 主要存放加载进来的类信息，包括方法，属性，常量池等，满了之后会引起out of memory错误。

## 针对HotSpot VM的实现

> HotSpot VM就是目前`sun SDK` `open JDK`自带的虚拟机



**Partial GC:并不收集整个GC堆的模式**

- Young GC：只收集young gen的GC
- Old GC：只收集old gen的GC。只有CMS的concurrent collection是这个模式
- Mixed GC：收集整个young gen、以及部分old gen的GC。只有G1有这个模式

**Full GC：收集整个堆，包括young gen、old gen、perm gen（如果存在的话）等所有部分的模式**

Major GC通常是跟full GC是等价的，收集整个GC堆。但是因为名词混乱的问题`major GC`也有可能指old GC。

 ### 分代式GC策略 触发条件

- young GC:当young gen中的Eden区分配满的时候触发。注意young GC中有部分存活对象会晋升到old gen，所以young GC后old gen的占用量通常会有所提高。
- full GC:当准备要触发一次young GC时，如果发现统计数据说之前young GC的平均晋升大小比目前old gen剩余空间大，则不会触发young GC而是转为触发full GC（因为HotSpot VM的GC里，除了CMS的concurrent collection之外，其它能收集old gen的GC都会同时收集整个GC堆，包括young gen，所以不需要事先触发一次单独的young GC）或者，如果有perm gen的话，要在perm gen分配空间但已经没有足够空间时，也要触发一次full GC;或者 `System.gc()`、`heap dump`带GC，默认也是触发full GC。

## 链接

[简书-浮生岁月](https://www.jianshu.com/p/5c677880a1b5)

[知乎-RednaxelaFX](https://www.zhihu.com/question/41922036)

