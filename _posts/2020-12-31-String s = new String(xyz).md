---
layout:     post                    # 使用的布局（不需要改）
title:      String s = new String("xyz")           # 标题 
subtitle:    Java
date:       2020-12-31              # 时间
author:     LSQ                      # 作者
# header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Java
---

&emsp;&emsp;
# String s = new String("xyz")
## 从一道面试题说起
&emsp;&emsp;**String s = new String("xyz"); 创建了几个实例？**  
&emsp;&emsp;这是一道很经典的面试题，在一本所谓的Java宝典上，我看到的“标准答案”是这样的：  
&emsp;&emsp;**两个，一个堆区的“xyz”，一个栈区指向“xyz”的s。**  
&emsp;&emsp;这个所谓的“标准答案”槽点更多，后面我们会慢慢分析。  
&emsp;&emsp;虽然答案很离谱，但是我觉得这个问题本身也不具有什么意义，因为问题没有既定义“创建”的具体含义，又没有指定“创建”的时间，是运行时吗？包不包括类加载的时候？有没有上下文代码语境？也没有定义实例是指什么实例，是指Java实例吗？还是单指String实例？包不包括JVM中的C++实例？  
&emsp;&emsp;显然，这个问题是一个“有问题的问题”。这个答案也是一个“有问题的答案”。  
## String结构
&emsp;&emsp;在分析之前，为了能更好的理解后面的知识点，我们需要对Java中的String结构有一个大致了解：  
![Java String结构](https://img-blog.csdnimg.cn/20201231143007771.png)
&emsp;&emsp;从上图可以看出，String类有三个属性：  
 - value：char数组，用于用于存储字符
 - 缓存字符串的哈希码，默认为0
 - serialVersionUID：序列化用的  
## 正常的问题与“合理的解释”
&emsp;&emsp;在上面的题干上加上"String"限定词，可以得到一个比较合理的问题:  
&emsp;&emsp;**String s = new String("xyz");创建几个String实例？**  
&emsp;&emsp;对于这个问题，网上也有很多错误的答案和解析，我认为这个答案看起来比较合理：  
&emsp;&emsp;**两个，一个是字符串字面量"xyz"所对应的、存在于全局共享的常量池中的实例，另一个是通过new关键字创建并初始化的、内容（字符）与"xyz"相同的实例。如果常量池中如果已经存在这个字符串，就只会创建一个。同时在栈区还会有一个对new出来的String实例的引用s。**  
&emsp;&emsp;考虑到了栈与堆，提到了常量池，我认为这已经达到大部分面试官对这个题目答案的期许了，或许这也是面试官想要考察的点。  
&emsp;&emsp;但这个答案也仅是比较合理，并不完全正确。  
&emsp;&emsp;首先，我不理解的是为什么很多答主总是用“常量池”来代替“字符串常量池”，在Java体系中，其实是有三个常量池的，三个常量池的概念和用处都不相同，混淆在一起容易给别人造成误解。  
&emsp;&emsp;其次，就算答主说的“常量池”就是“字符串常量池”，可“字符串常量池”中存的是String实例的引用，而不是字符串，这是有很大区别的。而且这个答案是没有考虑代码执行的环境。  
&emsp;&emsp;这些问题，下面都会一一分析。  
## 分清变量和实例
&emsp;&emsp;我们先回到开头的问题与“标准答案” ：  
&emsp;&emsp;**String s = new String("xyz"); 创建了几个实例？**  
&emsp;&emsp;**两个，一个堆区的“xyz”，一个栈区指向“xyz”的s**  
&emsp;&emsp;很明显写答案的人没有把变量和实例分清楚。在Java里，变量就是变量，类型的变量只是对某个对象实例或者null的，不是实例本身。声明变量的个数跟创建实例的个数没有必然关系。  
&emsp;&emsp;举个例子：  
```java
String s1 = "xyz";  
String s2 = s1.concat("");  
String s3 = null;  
new String(s1);  
```
&emsp;&emsp;这段代码会涉及3个String类型的变量： 
 - s1，指向下面String实例的1 
 - s2，指向与s1相同
 - s3，值为null，不指向任何实例  

&emsp;&emsp;以及3个String实例：
 - "xyz"字面量对应的驻留的字符串常量的String实例
 - ""空字符串字面量对应的驻留的字符串常量的String实例
 - 通过new String(String)创建的新String实例，没有任何变量指向它

## 类加载
&emsp;&emsp;对于String s = new String("xyz");创建几个String实例？这个问题。  
&emsp;&emsp;似乎网上的所有答案都把类加载过程和实际执行过程合在一起分析的。看起来是没有什么问题的，因为想要执行某个代码片段，其所在的类必然要被加载，而且对于同一个类加载器，最多加载一次。  
&emsp;&emsp;但是我们看一下这段代码的字节码：  
![字节码](https://img-blog.csdnimg.cn/20201231150149909.png)
&emsp;&emsp;字节码中似乎只出现了一次new java/lang/String，也就是只创建了一个String实例。也就是说原问题中的代码在每执行一次只会新创建一个String实例。  
&emsp;&emsp;这里的ldc指令只是把先前在类加载过程中已经创建好的一个String实例（"xyz"）的一个引用压到操作数栈顶而已，并没有创建新的String实例。  
&emsp;&emsp;不是应该有两个实例吗？还有一个String实例是在什么时候创建的呢？  
&emsp;&emsp;还有一个String实例在类加载的时候创建。  
&emsp;&emsp;我们都知道类加载的解析阶段是Java虚拟机将常量池内的符号引用替换为直接引用的过程，根据JVM规范，符合规范的JVM实现应该在类加载的过程中创建并驻留一个String实例作为常量来对应"xyz"字面量，具体是在类加载的解析阶段进行的。这个常量是全局共享的，只在先前尚未有内容相同的字符串驻留过的前提下才需要创建新的String实例。   
&emsp;&emsp;所以你可以理解成：  
&emsp;&emsp;在类加载的解析阶段，其实已经创建了一个String实例，执行代码的时候，又new了一个String实例。  
## JVM的优化
&emsp;&emsp;以上讨论都只是针对规范所定义的Java语言与Java虚拟机而言。概念上是如此，但实际的JVM实现可以做得更优化，原问题中的代码片段有可能在实际执行的时候一个String实例也不会完整创建（没有分配空间）。   
&emsp;&emsp;不结合上下文代码来看就直接说是“标准答案”就是耍流氓。  
&emsp;&emsp;我们看下这段代码  
![](https://img-blog.csdnimg.cn/20201231150326512.png)
&emsp;&emsp;运行这段代码，会不断的创建String对象吃内存，然后频繁的造成GC。对于这个结论相信大家都没有意见。  
&emsp;&emsp;我们加上-XX:+PrintGC打印日志；加上-XX:-DoEscapeAnalysis关闭逃逸分析（JDK8默认开启此优化，我们先关闭）  
![](https://img-blog.csdnimg.cn/20201231151042559.png)
&emsp;&emsp;运行一下看看：  
![](https://img-blog.csdnimg.cn/20201231152246653.png)
&emsp;&emsp;结果确实如我们所料，不断的创建String对象吃内存，造成频繁GC。  
&emsp;&emsp;我们现在将**-XX:-DoEscapeAnalysis改成-XX:+DoEscapeAnalysis**，重新跑一下这段代码:  
![](https://img-blog.csdnimg.cn/20201231152345675.png)
&emsp;&emsp;神奇的事情发生了，继续跑下去也没有再打出GC日志了。难道新创建String对象都不吃内存了么？   
&emsp;&emsp;实际情况是：经过HotSpot VM的的优化后，newString()方法不会新创建String实例了。这样自然不吃内存，也就不再触发GC了。   
&emsp;&emsp;现在再来看开篇的那个问题，不结合具体情况，还能简单的说String s = new String("xyz");会创建两个String实例吗？  
&emsp;&emsp;我只是举了一个逃逸分析的例子，HotSpot VM还有很多像这样的优化，比如方法内联、标量替换和无用代码削除。这些都会影响对象的创建的。  
## klass-oop
&emsp;&emsp;如果题干上没有加上“Java”实例的定语，那JVM中的oop实例我们也不应该忽略。  
&emsp;&emsp;为了后面能更好的说清楚这一点，先大致的介绍一下klass-opp模型。先做一个约定，全文只要涉及JVM具体实现的内容都是基于Jdk8中HotSpot VM展开的。  
&emsp;&emsp;HotSpot VM是基于C++实现，而C++是一门面向对象的语言，本身是具备面向对象基本特征的，所以Java中的对象表示，最简单的做法是为每个Java类生成一个C++类与之对应。但HotSpot VM并没有这么做，而是设计了一套klass-oop模型。  
&emsp;&emsp;klass，它是Java类的元信息在JVM中的存在形式。一个Java类被JVM类加载器加载之后，就是以klass的形式存在于JVM之中。  
![](https://img-blog.csdnimg.cn/20201231152706390.png)
&emsp;&emsp;oop，它是Java对象在JVM中的存在形式。每创建一个新的对象，在JVM内部就会相应地创建一个对应类型的OOP对象。  
&emsp;&emsp;其中instanceOopDesc表示非数组对象；  
&emsp;&emsp;arrayOopDesc表示数组对象；  
&emsp;&emsp;而objArrayOopDesc表示引用类型数组对象；  
&emsp;&emsp;typeArrayOopDesc表示基本类型数组对象。  
&emsp;&emsp;举个例子：Java中String类的一个实例，在JVM中会有一个对应的instanceOopDesc实例。  
![](https://img-blog.csdnimg.cn/20201231152919129.png)
## 字符串常量池
&emsp;&emsp;在Java体系中，有三种常量池：

 - class字节码中的常量池：存在于硬盘上。主要存放字面量和符号引用。
 - 运行时常量池：方法区的一部分。我们常说的常量池，就是指这一块区域。
 - 字符串常量池：存在于堆区。这个常量池在JVM层面就是一个StringTable，只存储对java.lang.String实例的引用，而不存储String对象的内容。一般我们说一个字符串进入了字符串常量池其实是说在这个StringTable中保存了对它的引用，反之，如果说没有在其中就是说StringTable中没有对它的引用。

&emsp;&emsp;今天，我们要了解的是字符串常量池。  
&emsp;&emsp;字符串常量池，即String Pool。在JVM中对应的类是StringTable，底层实现是一个Hashtable。利用的是哈希思想。  
![](https://img-blog.csdnimg.cn/20201231153042363.png)
&emsp;&emsp;看一段往字符串常量池添加字符串引用的方法：  
![](https://img-blog.csdnimg.cn/20201231153058894.png)
&emsp;&emsp;上面面这段代码虽然是C++写的，但我相信学过Java的人都能看懂，至少也能明白这段代码干了什么事情。无非是通过String的内容+长度生成的hashValue值定位下标index，然后将Java的String类的实例对应的instanceOopDesc封装成HashtableEntry作为存储结构存储到常量池。  
&emsp;&emsp;补充完字符串常量池的知识之后，我们再回到文章开头的那一题：  
&emsp;&emsp;String s = new String("xyz");创建了几个实例？  
&emsp;&emsp;如果包括JVM中的C++实例的话，有两个Java的String实例，两个String实例对应的instanceOopDesc实例，还有一个char[]数组对应的typeArrayOopDesc实例，加一起一共是5个。也可以说2个String实例加上3个oop实例。  
&emsp;&emsp;不理解的可以看下面这张内存图（图中省略了两个String对应的instanceOopDesc实例）。  
![](https://img-blog.csdnimg.cn/20201231153142141.png)
## 总结
&emsp;&emsp;String s = new String("xyz"); 创建了几个实例？  
&emsp;&emsp;通过以上的分析，我们会发现，每在这道题目的题干上每加一个定语，这道题目就会有不同的答案。是否考虑类加载过程，是否考虑JVM优化，是否包括对应的oop实例等等等等，都会有不同的答案。
     