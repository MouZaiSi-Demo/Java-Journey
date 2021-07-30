## Java异常

#### 大纲

- 为什么需要异常
- 异常的处理机制
- 常见的异常
- 异常的分类
- 异常处理步骤
  - throw try catch finally throws
- 自定义异常
  - 示例1
  - throws常见错误解释
  - public Exception(String message)含义
  - 异常与重写的关系
- 异常的优缺点

---

#### 为什么需要异常

- 有一些情况, `if, else`也就是逻辑是无法解决的, 这是因为java的抽象性

  ```java
  package a;
  
  public class A
  {
      int divide(int a, int b)
      {
          int m;
          m = a / b;
          return m;
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  import java.util.InputMismatchException;
  import java.util.Scanner;
  
  public class TestExcep_3
  {
      public static void main(String[] args)
      {
          int i;
          Scanner sc = new Scanner(System.in);
          try
          {
              i = sc.nextInt();
              System.out.printf("%d", 2 * i);
          }
          catch (InputMismatchException e)
          {
              System.out.printf("输入数据不合法 , 程序被终止!\n");
          }
  
      }
  }
  ------------------------------------------------------------------------------
  如果输入非数字, 会被捕获异常, 输出:输入数据不合法 , 程序被终止!
  反之, 则正常输出输入的两倍的值
  ```

---

#### 异常的处理机制

##### 什么是异常

- 异常(Exception)是程序运行过程中发生的事件, 该事件可以中断程序指令的正常执行流畅

- 当java程序运行时出现问题时, 系统会自动检测到该错误, 并立即生成一个与该错误对应的异常对象
- 然后把该异常对象提交给java虚拟机
- java虚拟机会自动寻找相应的处理代码来处理这个异常, 如果没有找到, 则由java虚拟机做一些简单的处理后, 程序被强行终止
- 程序员可以自己编写代码来捕捉可能出现的异常, 并编写代码来处理相应的异常



- **异常的报错**

  ```java
  package a;
  
  public class A
  {
      int divide(int a, int b)
      {
          return a / b;
      }
  
      public void f()
      {
          g();
      }
  
      public void g()
      {
          divide(6, 0);
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class TestExcep_3
  {
      public static void main(String[] args)
      {
          new A().f();
  
      }
  }
  ------------------------------------------------------------------------------
  结果:
  Exception in thread "main" java.lang.ArithmeticException: / by zero
  	at a.A.divide(A.java:7)// 可以明显看出有[栈]的味道
  	at a.A.g(A.java:17)
  	at a.A.f(A.java:12)
  	at a.TestExcep_3.main(TestExcep_3.java:10)
  ```



- **printStackTrace(), 这个函数是用来调试的, 可以看到栈的路径**

  ```java
  package a;
  
  public class A
  {
      int divide(int a, int b)
      {
          return a / b;
      }
  
      public void f()
      {
          g();
      }
  
      public void g()
      {
          divide(6, 0);
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class TestExcep_3
  {
      public static void main(String[] args)
      {
          try
          {
              new A().f();
          }
          catch (Exception e)
          {
              e.printStackTrace();
          }
      }
  }
  ```



---

#### 常见的异常

- 空指针异常

  ```java
  package aa;
  
  public class a
  {
      public static void main(String[] args)
      {
          Person p = null;
          System.out.println(p.age);
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class TestExcep_3
  {
      public static void main(String[] args)
      {
          try
          {
              new A().f();
          }
          catch (Exception e)
          {
              e.printStackTrace();
          }
  
      }
  }
  ------------------------------------------------------------------------------
  结果:
  java.lang.ArithmeticException: / by zero
  	at a.A.divide(A.java:7)
  	at a.A.g(A.java:17)
  	at a.A.f(A.java:12)
  	at a.TestExcep_3.main(TestExcep_3.java:9)
  ```

- 下标越界异常

- 算术异常



---

#### 异常的分类

- Error 是系统错误, 程序员无法处理这些异常
  - 由java虚拟机生成并抛出, 包括动态链接失败, 虚拟机错误等, Java程序无法对此错误进行处理
- Exception是程序员可以捕获并处理的异常
  - 一般程序中可预知的问题, 其产生的异常可能会带来意想不到的结果, 因此, Java编译器要求java程序必须捕获或声明所有的非运行时异常
- RuntimeException 是Exception的子类异常, 是你可以处理也可以不处理的异常
  - Java虚拟机在运行时产生的错误, 如被0除等, 其产生比较繁琐, 处理麻烦, 对程序可读性和运行效率影响太大. 因此由系统检测, 用户可不做处理
- 凡是继承自 Exception 但又不是 RuntimeException子类的异常我们都必须得捕获并进行处理



---

#### 异常的处理步骤

- **要抛出的异常 必须得是Throwable的子类(也可以是别的子类, 反正捕获的时候要符合多态的规则), 这个需要特别注意, 否则可能产生无法捕获的问题**

  ```java
  package a;
  
  import java.io.IOException;
  
  public class A extends Throwable// 成为Throwable的子类, 这样就不会报错了
  {
      public void f()
      {
  
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class B
  {
      public void f() throws A//把这个报错抛到上一层
      {
          throw new A();
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  import java.io.IOException;
  
  public class TestExcep_3
  {
      public static void main(String[] args)
      {
  
      }
  }
  ```

  

两种方式

- try catch语句, (finally)

- throws

  ```java
  package a;
  
  import java.io.IOException;
  
  public class A
  {
      public void f() throws IOException
      {
              throw new IOException();
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class TestExcep_3
  {
      public static void main(String[] args)
      {
          A aa = new A();
          //aa.f();
      }
  }
  ------------------------------------------------------------------------------
  结果是 退出代码为0, 也就是无报错, 如果aa.f()没被注释会报错: java: 未报告的异常错误java.io.IOException; 必须对其进行捕获或声明以便抛出, 因为在main里面没有进行捕获
  ```

- 需要注意的是, **throws抛出的是对栈的上一层抛出**





- ##### finally的作用

  - 无论`try`所指定的程序块中是否抛出异常, 也无论catch语句的异常类型是否与所抛弃的异常的类型一致, finally中的代码一定会执行
  - **finally语句为异常处理提供一个统一的出口, 使得在控制流程转到程序的其他部分以前, 能够对程序的状态作统一的管理**
  - 通常在finally语句中可以进行资源的清除工作, 如关闭打开的文件, 删除临时文件等



- ##### throws

  ```java
  void f() throws A
  {
  	...
  }
  ```

  - throws A表示调用f方法时f方法<strong style="color:red;">可能</strong>会抛出A<strong style="color:red;">类</strong>异常, 建议调用方法时, 最好对f方法可能抛出的A类异常进行捕捉
  - throws A 不表示f方法一定会抛出A类异常
  - throws A, f方法也可以不抛出A类异常
  - throws A 不表示调用f方法时, 必须的对A异常进行捕捉
    - 假设A是RuntimeException子类异常
    - 由于RuntimeException的子类异常可以处理也可以不处理, 所以编译器允许你调用f方法时, 对f方法抛出的RuntimeException子类异常不进行处理
  - 强烈建议
    - 对throws出的所有异常进行处理
    - 如果一个方法内部已经对A异常进行了处理, 则就不要再throws A




- ##### catch

  - 所有的catch只能有一个被执行

  - 有可能所有的catch都没执行

  - 先catch子类异常再catch父类异常

    - 原因是如果先catch父类异常, 这个父类异常会捕获子类异常(多态), **导致你写的捕获子类异常失效**

  - catch和catch之间是不能有其他代码的

  - 重写方法抛出异常的范围不能大于被重写方法排除的异常范围

    

---

#### 自定义异常(一般不自己定义, 学校叫你自定义属实有意思)

- 示例1

  ```
  package a;
  
  public class A
  {
      public int divide(int a, int b) throws DivisorIsZeroException
      {
          int m = 0;
  
          if(b == 0)
              throw new DivisorIsZeroException("除数不能为零!\n");
          else
              m = a / b;
  
          return m;
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class DivisorIsZeroException extends Exception
  {
      public DivisorIsZeroException(String name)
      {
          super(name);// 调用Exception的构造函数
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class TestExcep_7
  {
      public static void main(String[] args)
      {
          A aa = new A();
  
          try
          {
              aa.divide(6, 0);
          }
          catch (Exception e)
          {
              e.printStackTrace();
          }
      }
  }
  ------------------------------------------------------------------------------
  结果:
  a.DivisorIsZeroException: 除数不能为零!
  
  	at a.A.divide(A.java:10)
  	at a.TestExcep_7.main(TestExcep_7.java:11)
  ```


---

#### 特殊的点



##### throws和继承和重写

实例:

- 重写方法抛出异常的范围不能大于被重写方法排除的异常范围
  - 子类抛出异常的范围要比父类小, 这是为了确保**多态**的实现

```java
package aa;

public class A extends Exception
{
}
------------------------------------------------------------------------------
package aa;

public class B extends Exception
{
}
------------------------------------------------------------------------------
package aa;

public class C extends Exception
{
}
------------------------------------------------------------------------------
package aa;

public class M
{
    void f() throws A, B
    {

    }
}
------------------------------------------------------------------------------
package aa;

public class N extends M
{
    void f() throws A, B// 这里也可以只打开抛出A或B的出口, 也可以不打开出口
    {

    }
}
------------------------------------------------------------------------------
package aa;

public class Test
{
    public static void main(String[] args)
    {
        M m = new M();
        N n = new N();
    }
}

```



---

#### 异常的优缺点



- 以常规方法处理错误存在的问题
  - 编写程序大部分精力花在出错处理上了
  - 只把能够想到的错误考虑到，对以外的情况无法处理
  - 程序可读性差，大量的错误处理代码混杂在程序中
  - 出错返回信息量太少，无法更确切的了解错误状况或原因



- 异常的优缺点
  - 优点
    - 强制程序员考虑程序的安全性与健壮性
    - 增强了程序员对程序的可控性
    - 有利于代码的调试
    - 把错误处理代码从常规代码中分离出来
  - 注意
    - 异常并不一定能够使程序的逻辑更清晰
      - 因为有时我们必须得编写代码捕捉异常，所以可能会导致程序的逻辑非常混乱
    - 异常并不能解决所有的问题
