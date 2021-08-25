## Java集合（2） 之 Iterator接口详谈

咱们是咱们**[上一篇](https://zhuanlan.zhihu.com/p/209046075)**说到，

所有的集合都实现了Iterator（除Map集合外），

那咱们今天就一起来探讨一下，

Iterator到底有哪些不可告人的小秘密？

------

## 源码粗览

首先我们先进入到Iterator的源码类，

如下图示：

![img](https://pic3.zhimg.com/v2-dbc752f4c0598a4a70282ef5490407e6_r.jpg)

我们发现此类的内容并不多，

只有四个函数，分别为：

**hasNext、**

**next、**

**remove、**

**forEachRemaining**(JDK1.8及以后版本增加),

那么我们先大体做个介绍，

先混个脸熟，

知道这几个货到底是干嘛的，

大概有个了解，

后面我们详细的说明，

先看源码对hasNext的说明，

如下图示：

![img](https://pic1.zhimg.com/v2-b78d12fcbcddc365d937e68362d54754_r.jpg)

```text
如果容（迭代）器中有更多的元素，则返回true。
```

然后是**next**,

如下图示：

![img](https://pic1.zhimg.com/v2-a296b536453ae04ba8e907c5f3107368_r.jpg)

```text
返回容（迭代）器中下一个元素
```

并告知我们

```text
如果容器里没有更多的元素，
则会抛出NoSuchElementException异常,
```

再来看看**remove**，

发现不对啊，

这里不是接口类吗？

这货怎么具备方法体呀？

关于这个问题其实咱们在**[这篇文章](https://zhuanlan.zhihu.com/p/212819528)**也提过一嘴，

从JDK1.8开始接口允许定义默认方法和静态方法的新特性，

这里不过多赘述咱们往下接着说，

如下图示：

![img](https://pic4.zhimg.com/v2-85a4d77294658fa6633228c064813c17_r.jpg)

```text
从集合中移除本迭代器返回的最后元素…
```

并且使用不当很容易抛出异常，

例如：

![img](https://pic3.zhimg.com/v2-d44c33f7ab0495ff0fdc3a0b0518069a_r.jpg)

```text
如果此Iterator不支持remove操作时
会抛出UnsupportedOperationException异常
```

再例如：

![img](https://pic3.zhimg.com/v2-8ff10194c6b4e5bd5fe0cf3392f70122_r.jpg)

```text
如果next方法没有被调用，
或者在上次调用next方法之后已经调用了remove方法时，
会抛出IllegalStateException异常
```

最后我们定位到默认方法**forEachRemaining**，

如下图示：

![img](https://pic4.zhimg.com/v2-edc5ca03a51b33506a39593e5975121f_r.jpg)

```text
对（容器中的）剩余的每个元素执行操作，
直到所有元素都已被处理或抛出异常…..
```

*这里重点的就是**剩余元素**（remaining element）先make一下，*

*后面会详细说明;*

------

## **来个栗子**

简单扫了一眼之后知道它们分别是干嘛用的，

那么到底怎么用呢？

我们写用例来进一步说明，

先创建一个集合并赋值，

最后打印咋控制台，

如下图示：

![img](https://pic4.zhimg.com/v2-7ad9020654199bc078dc418863702c33_r.jpg)

接下来我们获取该集合**strs**的迭代器，

并调用**next、和hasnext**方法，

最后将值打印在控制台，

如下图示：

![img](https://pic4.zhimg.com/v2-f4483bf858892c60a4d84f3b404219e3_r.jpg)

------

**hasNext分析**

再咱们第一部分探讨的基础上，

我们知道如果容器里还要更多的元素将返回true，

道理是没有错的，

很好奇方法**hasNext**在怎么实现的？

我们例子用的ArrayList类，

那么不用说也知道进到此类先看看再说，

一阵搜索后，

如下图示：

![img](https://pic1.zhimg.com/v2-d5a9dcd2a7ef677048a6033936626c9c_r.jpg)

我们发现在ArrayList的**内部类**，

对其进行了**重写**，

大致理解下是 ：

```text
下一个元素的索引(index)值若不等于此容器的size，
意思还没有到集合的末尾；
即 返回true；
```

小贴士：

![img](https://pic2.zhimg.com/v2-1669094e97ae08340f160f51ca7761e1_r.jpg)

```text
cursor：表示下一个元素的索引
lastRet: 表示上一个索引的索引
expectedModCount：表示对此集合修改次数的期望值，初始值为modCount。
那么modCount又是个啥？
```

![img](https://pic4.zhimg.com/v2-12ac6df2955343563a34111d94a012df_r.jpg)

```text
再继续深入源码我们发现modCount是AbstractList的成员变量，
该值表示的是list的修改次数，
单次调用add()或者remove()时会对此字段进行+1操作。
咱们看下源码的调用过程，
大致如下：
```

![img](https://pic2.zhimg.com/v2-ae2f2d9b7cbba5f0d166db2ae39cd945_r.jpg)

回到我们代码的例子，

我们知道集合的容量我们设置的是10，

迭代器调用**hasNext()**时，

因为是第一次不能可能超出集合的**size**,

**返回true**属实符合我们的预期。

如下图示：

![img](https://pic4.zhimg.com/v2-24e5d4032a9debf510aa93b2e48b435f_r.jpg)

------

## **next分析**

接下来我们看看**next**方法，

如下图示：

![img](https://pic3.zhimg.com/v2-9a70a4df05d62162ce3a5bd11cfae79a_r.jpg)

让我们解读一下：

我们看到程序会先调用**checkForComodification**方法，

如下图示：

![img](https://pic2.zhimg.com/v2-60ea7d6100f38bacd697717b6825b2b5_r.jpg)

```text
我们从上文的小贴士可以知道，
此时的modCount和expectedModCount值都为0，
所以不会抛出ConcurrentModificationException异常
```

我们回到主线上来，

然后根据**cursor**的值获取到元素，

接着将**cursor**的值赋给局部变量i，

并判断集合中没有更多的值（**i**）会抛出**NoSuchElementException**异常。

接着是将存储ArrayList数组的**缓冲区**，

赋值（ArrayList的容量就是**缓冲区的长度**）给**elementDate**数组，

并判断当前的索引值（**i**）在elementDate缓冲区的容量内（不包含），

否则抛出**ConcurrentModificationException**异常。

重点来啦，

若在其范围内的话，

**下一个索引**（cursor）值将会增加1个，

也就是在自身基础上（**i**） +1;

然后将i赋值给**lastRet**，

也就是**未+1**前得cursor的值，

![img](https://pic3.zhimg.com/v2-5d6b47896df1da0ad8f167f2eb801e6e_b.png)



那么此时的**lastRet**（上一个索引）就变成原来（**未+1前**）的**cursor**（下一个索引）了，

也是方法内的局部变量【**i**】，

相当于此时的**next指针**在集合中的位置向下移动了一次，

也就是从开始的初始值【**lastRet = - 1**】变成了【**lastRet = 0**】

那咱们刚写的代码为例：

如下图示：

![img](https://pic3.zhimg.com/v2-bcb66b14baad3e7b2bf36030b0d0746a_r.jpg)

输出【坐标0】就符合我们预测啦。

------

## **remove分析**

那么**remove**的，

我们调整下用例代码，

先调用**remove方法**移除掉集合中的**第一个**元素，

然后在重新打印一下集合**strs**的内容，

如下图示：

![img](https://pic2.zhimg.com/v2-e2eec967bece9cd70f5c11a487df1305_r.jpg)

咱们执行看结果，

发现抛出异常啦，

如下图示：

![img](https://pic1.zhimg.com/v2-c47cf47a4da66c255790f5db39696fec_r.jpg)

这个错误很熟悉，

因为我们在上文**Iterator**的源码中已经看到过啦，

再来看一眼（别往上划啦，往下看吧），

如下图示：

![img](https://pic4.zhimg.com/v2-1c3569af8893eaa9f0fd91f32be21a6f_r.jpg)

直译过来的意思意思一点都不亲民，

咱们再解释下，

在两种情况下报这个错误：

1. *没调用next，直接调用了remove（也就是我们代码用例的这种用法）；*
2. *调用了next，接着调用一次remove后又调用了一次remove；*

第二种情况的代码用例截图，

如下图示：

![img](https://pic1.zhimg.com/v2-3bf38fa9b5a60c7f83a3a106db5d6f8c_r.jpg)

不理解吗，

大爷的...我也不懂，

来、来、来…看看源码，

如下图示：

![img](https://pic3.zhimg.com/v2-dcf3175d813807435e37eb20e7dd23da_r.jpg)

我去…

第一行代码就是判断**lastRet**是否**小于0**，

我们知道这货的初始值是【**-1**】啊，

可不直接就进了异常里了嘛！

我们再往下看，

调用next后该删(**lastRet**)的元素被**remove**掉，

然后**下一个索引**（cursor）继承了**上一个索引**(lastRet)啦，

也就是我们刚**删掉的元素**（索引为值0），

而此时当前的**上一个索引**（lastRet）又重新赋值为**-1**啦。

所以我们调**用完next**后**重复两次调用remove**，

第二次同样会进入到开始的判定里，

也就是进入到异常里去啦。

所以正常的用法应该，

如下图所示：

![img](https://pic3.zhimg.com/v2-9ad32c22b119bb364567f69eba3c476e_r.jpg)

我们再来验证下，

还是以上文的代码为例，

这次**remove**掉【坐标5】，

代码应该怎么样做调整呢？

如下图示：

![img](https://pic1.zhimg.com/v2-78abdc364ffeab823201e4c4b0cb2300_r.jpg)

通过以上截图的代码，

我们要删除【坐标5】，

需要调用**next**的6次才能定位到【光标5】，

为了让大家更直观的理解，

本着灵魂画师的执念，

粗略的绘制了代码执行图，

如下图示：

![img](https://pic4.zhimg.com/v2-98a8f74cf35d24378032bcb49d4b95b7_r.jpg)

从上图很明显的可以看出来，

想要删除掉集合中的元素【坐标5】，

必须先用调用next**越过将要删除**的【坐标5】；

说明一下咱们上文的例子，

是为了给同学们提供用例进行展示，

实际使用时不会那么用，

这里还是提供一个例子给大家作参考，

需求与上文一样，

代码截图如下：

![img](https://pic3.zhimg.com/v2-d6f1dda512c94daea5b67a0d23eb0096_r.jpg)

根据实际需求，

大家要学会灵活运用；

------

## **forEachRemaining分析**

说到**forEachRemaining**咱们从方法名称也能理解个大概，

看到**forEach**不用猜也知道是遍历集合数据的，

方法的后缀使用**Remaining**[剩下\余的]，

就有点不好理解啦，

那么什么叫遍历剩余元素呢？

接下来咱们就把上文挖的坑给填上，

先来看下它的源码，

如下图示：

![img](https://pic3.zhimg.com/v2-9f66de970a7f7dd28fa197cf742ce056_r.jpg)

源码中其他的部分我们在上文都有探讨过，

这里就不具体说明啦，

这里最重要的部分就是**while**循环，

循环的条件是【i】（下个索引的值）不等于**size**(ArrayList集合的大小),

然后使用**consumer.accept()**方法将输入的元素数据进行接收，

*小提示：*

```text
 这里的consumer接口表示一个接受单个输入参数并且没有返回值的操作。
用法涉及到拉姆达类似上文forEachRemaining(ele -> System.out.print(ele)) 这种。
```

废话不多说直接上代码，

逻辑很简单就是，

使用**while**循环打印出集合的数据，

如下图示：

![img](https://pic3.zhimg.com/v2-0942df528558deca5bb942988aab13b2_r.jpg)

我们看到结果数据符合我们的预期，

没有任何问题，

那么使用**forEachRemaining**呢？

如下图示：

![img](https://pic2.zhimg.com/v2-ec4f2eda170b99c43eb69635bb498809_r.jpg)

我们看到数据结果也在预料之内，

只是代码要比**while简单了**不少，

就这吗？

咱们带着疑问接这往下看，

将注释掉的代码放开，

咱们让其打印两遍集合中内容，

如下图示：

![img](https://pic4.zhimg.com/v2-070c76539b7c7484074aa0a59e5ce943_r.jpg)

我们发现不对啊

使用**forEachRemaining**没有打印出结果啊，

先别着急下结论，

我们调整下代码再来看效果，

调整后的重心是，

在**while**循环中设定一个条件，

集合中的数据轮询包含“**5**”的时候就**跳出**循环，

我们调整完在执行后看结果，

如下图示：

![img](https://pic4.zhimg.com/v2-09c34698e280cd815d8c740ab10897eb_r.jpg)

我们发现使用**forEachRemaining**的部分经过修改代码后，

打印出来啦，

打印出的数据就是**while**循环剩余（**Remaining**）的数据，

此时我们再来看源码的**while**部分，

如下图示：

![img](https://pic3.zhimg.com/v2-9e00327db9657520cb6c3fd02e430b1e_r.jpg)

我们用例中的while循环，

在轮询集合执行到索引为【**5**】时，

跳出了循环，

接着执行**forEachRemaining**方法，

下个索引（**cursor**）此时的值为【6】

进入到**forEachRemaining**中的**while**的【i】（**cursor**）**不等于size**(ArrayList集合的大小),

while也就接着**索引【6】开始执行**啦。

---

[田亦裔](https://www.zhihu.com/people/tian-yi-yi-77)

开发工程师

作者：https://www.zhihu.com/people/tian-yi-yi-77

原答案：https://zhuanlan.zhihu.com/p/226172908