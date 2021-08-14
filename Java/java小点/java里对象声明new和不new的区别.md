## java里对象声明new和不new的区别？，没new的类变量能不能调用该类的属性和方法？

new关键字，用于创建对象，对象之间数据不共通。

对象，是数据和对其操作的封装形式。

属性：含有get和set方法的字段。

字段：用来存放数据的，存在于一定范围内的变量，普通字段存在于一个对象内，静态字段存在于一个类的范围内。

```java
public class Test 
{

    private numA;

    private static int numB = 100;

    public int getNumberB() 
    {
    	return numB;
    }

    public static int getB() 
    {
        return numB;
    }

    public Test(int numA) 
    {
        this.numA = numA;
    }

    public int getNumber() 
    {
        return this.numA;
    }

    @Override
    public String toString() 
    {
        return this.numA + "";
    } 
}
```

我们现在有如上一个类，这里面有一些方法，看如下的例子：

```java
Test itemA = new Test(123);
Test itemB = new Test(456);

itemA.getNumber();
itemB.getNumber();
itemA.getNumberB();
itemB.getNumberB();

Test.getB();
```

以上，itemA的numA和itemB的numA是不一样的，他们相互不会有影响，这是因为他们是两个对象，而numA是属于对象的字段，属于一个类的不同对象，他们拥有相同的类型和数量的字段，但是字段的内容可以不同，就像itemA和itemB，他们属于Test，都有一个numA字段，但是numA的内容不同。

而我们看getNumberB呢？这个方法统一返回static的numB，这个字段标记有static，这意味着所有的Test类的对象都会共享这一个字段，他并不属于某个对象，而是属于这个类的所有对象，如果我们对他进行修改，那么将会干涉所有使用它的对象，例如我们把numB设置为200，那么itemA和itemB的getB()和getNumberB()都会返回200。

因此一般为了区分这种字段和在对象层面可以有内容不同的字段，我们一般不会写作getNumberB这种方法的形式，而是改用getB这种静态方法。

另外类也是对象。只是这种对象比较特殊，可以产生很多从属于他的对象，而类本身是只有一个的，直接使用类名代表，从面相对象的角度理解，调用类这个对象的方法，当然是通过对象名字直接调用，即类名加上点加上方法名这样子，那么为什么不需要自己去new一个类对象呢？因为这件事情类加载器替你做好了，他会把字节码载入内存，转化为字节码对象，然后从字节码得到类对象，而在这之后要怎么使用这个类，也需要你自己来把握，因此类的对象，你是需要new出来的。

再说new的具体作用，他会申请一块合适的内存，并且变成适合对象存放的样子，这也是为什么对象的字段可以互不影响的原因。

另外在运行的时候，java是不会去判断方法是否能正确完成的，一旦发现对象没有内存，即没有new，但是你调用了他的方法或者字段，那么就会直接报错，应该是NullPointerException，告知你对象不应该为空。

通常在java中，我们把为对象申请内存，并获取对象引用的过程叫做实例化，即：new是为了实例化一个对象。

那么除了new 是否存在其他实例化对象的方法呢？当然有，字节码对象的newInstance方法（类名.class.newInstance）可以直接创建对象，另外，此方法也存在于Constructor对象，即字节码对象的getConstructors可以得到构造方法对象，构造方法对象同样有newInstance方法可以使用，也能够完成实例化，而且部分类提供了工厂方法，通过工厂方法不使用new也可以获取对象的。

综上所述：

1.new和不new的区别：决定了是否能够操作对象层面的数据，即是否能够操作非static的字段和方法，非static方法只有对象能够使用。

2.没有new的类变量（类对象）能不能调用该类的属性和方法：可以，把类视作对象，那么该对象的字段是所有该类对象所共享的静态字段，该类对象的方法为该类所有对象共享的静态方法，类对象的名字即类名，在没有对象的情况下，能够调用静态方法和使用静态字段，即，能够使用类对象的方法和字段，即，能够使用类变量的方法和字段。

---

作者：https://www.zhihu.com/people/SWDCloud

原答案：https://www.zhihu.com/question/355478340/answer/911870793