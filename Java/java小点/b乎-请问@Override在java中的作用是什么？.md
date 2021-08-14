## 请问@Override在java中的作用是什么？

## 简述：

@Override是伪代码,表示重写(当然不写也可以覆盖)，但加上后会额外检测覆盖前后方法的参数类型是否相同。

### 写上主要有如下好处:

1、告诉读代码的人，这是一个复写的方法。可以当**注释用**,方便阅读；

2、帮助自己**检查**是否正确的复写了父类中已有的方法。编译器可以给你**验证**@Override下面的方法名是否是你父类中所有的，如果没有就算参数不匹配便报错。

当你如果没写@Override，而你下面的方法名又写错了，这时你的编译器是可以编译通过的，因为编译器以为这个方法是你的子类中自己增加的方法。

### 举例：

在重写父类的onCreate时，在方法前面加上@Override 系统可以帮你检查方法的正确性。

@Override

public void onCreate(Bundle savedInstanceState)

 {

…….

}

这种写法是正确的，如果你写成：

@Override

public void oncreate(Bundle savedInstanceState)

 {

…….

}

编译器会报如下错误：

The method oncreate(Bundle) of type HelloWorld must override or implement a supertype method。

上述用以确保你正确重写onCreate方法（因为oncreate应该为onCreate）。

### 不加是怎么样的？

如果你不加@Override，则编译器将不会检测出错误，而是会认为你为子类定义了一个新方法：oncreate。



---

作者：https:undefined

原答案：https://www.zhihu.com/question/19803226/answer/781423787