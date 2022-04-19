# Hello JVM

#### **JVM的位置**

​	处于操作系统之上，使用C语言编写，jre中包含JVM。

​	硬件——>操作系统——>JVM

#### **JVM体系结构**

​	JVM定义：指以软件的方式模拟具有完整硬件系统功能、运行在一个完全隔离环境中的完整计算机系统 ，是物理机的软件实现。

​	![JVM图解](D:\SIAT\Hello JVM\JVM图解.png)

​	所谓得JVM调优：就是针对堆以及方法区进行调优

#### **类加载器**

​	加载class文件，

​	有多种加载器：虚拟机自带的加载器、启动类加载起、扩展类加载器、应用程序加载器

```java
Hello hello = new Hello();
        Class<? extends Hello> aClass = hello.getClass();
        ClassLoader classLoader = aClass.getClassLoader();
        System.out.println(aClass.hashCode());
        System.out.println(aClass);
        System.out.println(classLoader);
        System.out.println(classLoader.getParent());
        System.out.println(classLoader.getParent().getParent());
```



#### **双亲委派机制**

​	目的：保证安全

​	APP--->扩展类加载器--->根加载器  从根加载器进行类的加载 没有的话 再往后找

步骤：

 1. 类加载器收到类加载的请求

 2. 将这个请求向上委托给父类加载器去完成，一直向上委托，直到启动加载器

 3. 启动加载器检查是否能加载当前类，能加载就结束，否则抛出异常，使用子类加载器完成

    ![img](https://img-blog.csdnimg.cn/20201217213314510.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2NvZGV5YW5iYW8=,size_16,color_FFFFFF,t_70)

#### **沙箱安全机制**

​	沙箱机制主要是将java代码限定到JVM特定的区域运行。

​	现在的安全模型就是，jvm给不同的代码分配不同的域，该代码就拥有这个域所拥有的对于本地资源的全部权限。（域类似于角色）

​	沙箱安全机制的组成部分：1.字节码校验器：它保证java代码符合java语言规范，核心类由于已经校验过了封装好的，字节码不会校验核心类

​												2.类加载器，类加载器是利用了双亲委派机制，它保证了好的代码不会被坏的代码污染 它定义了被信任类库的边界 为代码归入域（类似于分配角色，分配权限）

#### **Native**

​	*凡是带了native关键字的，说明java的作用范围达不到了，会去调用底层c语言的库。*

​	*会进入本地方法栈，调用本地方法的本地接口 JNI*

​	*JNI作用：扩展java的使用，融合不同的编程语言为Java所用*       设计之初是为了调用C C++程序

​	因此开辟了本地方法栈，登记native方法，在最后总执行的时候·，加载本地方法库的方法通过JNI

#### **PPC寄存器**

​	每一个线程会有一个程序计数器，是线程私有的，类似于一个指针，所占用的内存非常小





#### **方法区**

​	所有线程共享，基本上所有定义方法的信息全都存于此。

​	==静态变量，常量，类信息，运行时的常量池在方法区中，但是实例变量存在于堆中==

​	static， final，Class， 常量池

#### **栈**

​	栈：后进先出

​	队列：先进先出（FIFO）

栈：栈内存，主管程序的运行，生命周期与线程同步

线程结束，栈内存就释放，对于栈来说，不存在垃圾回收的问题

一旦线程结束，栈就结束

栈：8大基本类型，对象引用，实例的方法



#### **三种JVM**

Sun 公司的 HotSpot（常用

BEA 公司 JRockit

IBEM 公司 J9VM

#### **堆**

​	Heap  堆中存放类的实例，一个JVM中只有一个堆，堆的内存大小是可以调节的

​	堆中分为三个区域：新生区，老年区，以及永久区

#### **新生区、老年区**

新生区分为：伊甸园区，幸存0区，幸存1区           轻gc

新生区是类诞生，成长甚至死亡的地方

伊甸园：所有的对象都是在伊甸园区诞生，

老年区：                                       重gc

gc垃圾回收主要是存在于伊甸园区与养老区

#### **永久区**

jdk8之后改名为元空间

元空间 逻辑上存在，物理上不存在

这个区域常驻于内存，用于存放JDK自身携带的Class对象，Interface元数据，存储的是Java运行时的一些环境或类信息  这个区域不存在垃圾回收。 关不JVM才会释放该区域的内存。



``` java
		//字节
        long max = Runtime.getRuntime().maxMemory();
        long total = Runtime.getRuntime().totalMemory();
        System.out.println(max+"       "+max/1024/104);
        System.out.println(total+"       "+total/1024/104);
```

默认情况下：分配的总内存为电脑内存的1/4  而初始化内存：1/64



#### **堆内存调优**

​	在遇到了OOM异常  1.先进行扩大堆内存，看结果   

​										2.使用插件工具进行调整

#### **GC 常用算法**

​	垃圾回收机制：

​		在堆和方法区才能进行垃圾回收，大部分都是在新生代进行 轻GC  

常见的GC算法、：

​		标记清除法   标记清除压缩法   复制算法   引用计数法

​			**复制算法：**GC复制算法是利用From空间进行分配的。当From空间被完全占满时，GC会将活动对象全部复制到To空间。当复制完成后，该算法会把From空间和To空间互换，GC也就结束了。From空间和To空间大小必须一致。这是为了保证能把From空间中的所有活动对象都收纳到To空间里。当进行到15次转换后，就会把from中的内容复制到老年区中



![GC复制算法](https://img-blog.csdn.net/20170404094959290?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxNDIyODM3NQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



​			好处：不会有内存碎片     缺点：会浪费一半的空间

​			复制算法最佳存活场景：对象的生存率较低

​	**标记清除法：**先进行标记，随后进行清除。对程序中的非活动对象进行标记，随后清除。通过这两个阶段令不能利用的空间重新得到利用。

标记阶段：就是对堆上的对象进行遍历并在遍历过程中对活动对象以及活动对象所能引用的对象进行活动状态标记。遍历阶段根据方式不同可以分为深度优先和广度优先，由于深度优先在内存的占用上比广度小，一般选择深度优先遍历。

清除阶段：这个阶段会对堆上的对象进行一次遍历（相较于标记阶段的深度优先更像遍历数组的模式）这期间会将非活动对象加入到空闲空间链表中作为空闲的块。这个阶段来说堆越大所耗费的时间也越长。

分配：在空闲空间链表中根据选择算法寻找合适的块分配空间并返回这个块的引用，如果找到的块过大就讲剩余的内存存回到空闲空间链表。

合并：根据分配阶段来看，标记清除算法的清除会带来许多的碎片化空间，一个简单的做法就是在清除阶段加入合并的操作（就是对地址是否相邻的一个if判断），对本就相邻的两个块区域合并为一个块。

缺点：费时，会产生内存碎片。

优点：不会需要额外的空间。

**总结：**

​	内存效率：复制算法>标记清除>标记压缩算法

​	内存整齐度：复制算法=标记压缩算法>标记清除算法

​	内存利用率：标记压缩算法=标记清除算法>复制算法



轻GC与重GC分别在什么情况下使用：

​	年轻代：

​		存活率低

​		复制算法！

​	老年代：

​		区域大：存活率高

​		标记清除（内存碎片不多时）+标记压缩算法 混合实现

​	







**GC的四大算法**
1.复制算法
 年轻代中使用的是Minor GC，这种GC算法采用的是复制算法(Copying)

JVM把年轻代分为了三部分：1个Eden区和2个幸存区（分别叫from和to）。一般情况下，新创建的对象都会被分配到Eden区(一些大对象特殊处理),这些对象经过第一次Minor GC后，如果仍然存活，将会被移到Survivor区。对象在Survivor区中每熬过一次Minor GC，年龄就会增加1岁，当它的年龄增加到一定程度时，就会被移动到年老代中。因为年轻代中的对象基本都是朝生夕死的(90%以上)，所以在年轻代的垃圾回收算法使用的是复制算法，复制算法的基本思想就是将内存分为两块，每次只用其中一块，当这一块内存用完，就将还活着的对象复制到另外一块上面。复制算法不会产生内存碎片。![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-wcVjvif7-1614257533348)(C:\Users\辛友\Pictures\Tyrope\GC复制.png)]](https://img-blog.csdnimg.cn/2021022520523079.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0lUUm9va2lFMQ==,size_16,color_FFFFFF,t_70#pic_center)



谁先清空，谁变成to区，GC的复制算法会一直重复这样的过程，直到“To”区被填满，“To”区被填满之后，会将所有对象移动到年老代中。因为Eden区对象一般存活率较低，一般的，使用两块10%的内存作为空闲和活动区间，而另外80%的内存，则是用来给新建对象分配内存的。一旦发生GC，将10%的from活动区间与另外80%中存活的eden对象转移到10%的to空闲区间，接下来，将之前90%的内存全部释放，以此类推。

2.标记清除算法
第一步：标记（Mark）
第二部：清楚（Sweep）：回收为被标记对象

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-NhAKQm62-1614257533351)(C:\Users\辛友\Pictures\Tyrope\标记清除算法.png)]](https://img-blog.csdnimg.cn/20210225205245359.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0lUUm9va2lFMQ==,size_16,color_FFFFFF,t_70#pic_center)

3.标记压缩(Mark-Compact)

![[外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-Bmoj24Vc-1614257533352)(C:\Users\辛友\Pictures\Tyrope\标记压缩.png)]](https://img-blog.csdnimg.cn/2021022520525692.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0lUUm9va2lFMQ==,size_16,color_FFFFFF,t_70#pic_center)


4.标记清除压缩(Mark-Sweep-Compact)
总结
内存效率：复制算法>标记清除算法>标记压缩算法（时间复杂度）

内存整齐度：复制算法=标记压缩算法>标记清除算法

内存利用率：标记压缩=标记清楚>复制算法

没有最好的算法，只有最合适的算法
1、年轻区域特点是区域相对老年代较小，对像存活率低

这种情况复制算法的回收整理，速度是最快的。复制算法的效率只和当前存活对像大小有关，因而很适用于年轻代的回收。而复制算法内存利用率不高的问题，通过hotspot中的两个survivor的设计得到缓解。

2老年代的特点是区域较大，对像存活率高

存在大量存活率高的对像，复制算法明显变得不合适。一般是由标记清除或者是标记清除与标记整理的混合实现。

算法明显变得不合适。一般是由标记清除或者是标记清除与标记整理的混合实现。

Mark阶段的开销与存活对像的数量成正比，这点上说来，对于老年代，标记清除或者标记整理有一些不符，但可以通过多核/线程利用，对并发、并行的形式提标记效率


#### **JMM  Java Memory Model**

​	1.什么是JMM java内存模型

作用：缓存一致协议，定义数据访问读写的规则



所有的共享变量都存储在主内存中共享，每个线程拥有自己的工作内存（相当于高速缓存，有利于提高访问速度），工作内存中保存的是主内存中变量的副本，线程对变量的读写操作是在自己的工作内存中进行,而不是直接读写主内存中的变量。如此多线程进行数据操作时，将可能发生线程安全问题，因此JMM需要提供原子性、可见性、有序性的保证。



![img](https://upload-images.jianshu.io/upload_images/17733727-9733fde7cf81d0a7.png?imageMogr2/auto-orient/strip|imageView2/2/w/471/format/webp)



对于JMM中可能会用到的关键字：

![img](https://upload-images.jianshu.io/upload_images/17733727-59cf1ccfb60071e6.png?imageMogr2/auto-orient/strip|imageView2/2/format/webp)

---



## **总结 summary**

