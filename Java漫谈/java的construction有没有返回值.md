今天在看了微软vs的对c++的constructor的debug的pattern初始化操作的时候突然想到之前java课上老师讲java的construtor有没有返回值, 老师说他认为有, 实际上我觉得完全可以去看java是如何实现的, 或者说看constructor是什么

---
**“Java构造方法有人说有返回值，有人说没有，难道就没人真正能给出正确答案吗？”**
这是类知乎的问题2333, 实际上可以自己去看应该是可以找到的, 毕竟Java已经20+岁了

### 开始查找
找到Oracle的[java8文档](https://docs.oracle.com/javase/specs/jls/se8/html/index.html)

之后点击[class member](https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.2)
规定了类的成员的来源继承自父类、继承自父接口、自身定义这3个来源，同时里面写到了这样的内容：

>Constructors, static initializers, and instance initializers are not members and therefore are not inherited.
构造方法，静态初始化器，对象初始化器，都不是类的成员，因此它们也不可以被子类继承。

然后再看[8.8. Constructor Declarations](https://docs.oracle.com/javase/specs/jls/se8/html/jls-8.html#jls-8.8)关于构造方法的定义有以下说明：

>a constructor declaration looks just like a method declaration that has no result
一个 constructor 的定义就像是一个没有返回值的 method 的定义。

**好吧, 这实际上是翻译问题**
官方用了三种定义:
- Filed：成员变量
- Method：普通方法
- Type：内部定义的其他类型，如内部接口、内部类等

而 **Constructor** 则是负责对一个对象进行初始化的，会在对象创建完成后由系统执行，它本身并不是类的成员，更不是一个“方法”，**Constructor** 的中文应该是构造器更合适。

构造方法是：Constructor，成员方法是：Method也就是说，它们两个是两个平等的概念，而不是包含的关系。

**既然Constructor 都不是个“方法”，那就没有返回值这个问题了**

---

**又想到, 老师给出的理由是如果**
```java
A a = new a();
```
a()会返回该空间地址给栈空间的a, 所以认为构造函数是有隐性的返回值, 反正不可能是显性的

而实际上, 这里造成误解的地方是 **=**, 实际上这里并不是赋值的意思, 而是**Definition:  将a定义为构造出的A对象**

这些都是**规范层面**, 实际上你完全可以理解为堆地址的到栈地址的赋值, 没有必要深究

---

>人生还是应该多多思考, 而非整日在寝室, ~~或参加无趣的学生会, 或打着无聊又有压力的劣质网游~~