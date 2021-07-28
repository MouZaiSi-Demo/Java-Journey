## 异常程序例子



- 示例1说明 **可以利用try catch来捕获异常, 此时, main不会停止程序, 因为这个异常对象被我们手动拦截了, 所以**

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
--------------------------
package a;

public class TestExcep_1
{
    public static void main(String[] args)
    {
        A aa = new A();

        try
        {
            aa.divide(6, 0);
        }
        catch (ArithmeticException e)
        {
            System.out.printf("除零错误, 你的程序出错了, 除数不能为零\n");
        }
        System.out.printf("main仍然在执行");
    }
}
----------------------------------------
输出结果: 
除零错误, 你的程序出错了, 除数不能为零
main仍然在执行
```



- 示例2, **main函数里面也没有拦截异常, 最终异常对象被抛到了java虚拟机里, 同时停止了程序并报错在命令行内**

  ```java
  package a;
  
  public class A
  {
      int divide(int a, int b)
      {
          int m;
          m = a / b;
          System.out.printf("divide后仍然执行");
          return m;
      }
  }
  ------------------------------------
  package a;
  
  public class TestExcep_1
  {
      public static void main(String[] args)
      {
          A aa = new A();
  
          aa.divide(6, 0);
  
          System.out.printf("main仍然在执行");
      }
  }
  -----------------------------------------
  运行结果:
  Exception in thread "main" java.lang.ArithmeticException: / by zero
  at a.A.divide(A.java:8)
  at a.TestExcep_1.main(TestExcep_1.java:9)
  ```



- 示例3 **说明如果程序有异常, 且没有被手动抓取, 异常发生之前的操作仍然会执行**

```java
package a;

public class A
{
    int divide(int a, int b)
    {
        int m;
        System.out.printf("divide异常发生前仍然执行\n");
        m = a / b;
        System.out.printf("divide异常发生后仍然执行\n");
        return m;
    }
}
------------------------------------------------------------------------------
package a;

public class TestExcep_1
{
    public static void main(String[] args)
    {
        A aa = new A();

        aa.divide(6, 0);

        System.out.printf("main仍然在执行");
    }
}
------------------------------------------------------------------------------
结果:
divide异常发生前仍然执行
Exception in thread "main" java.lang.ArithmeticException: / by zero
at a.A.divide(A.java:9)
at a.TestExcep_1.main(TestExcep_1.java:9)
```



- 示例4 说明 **放在`try`中的语句, 会具有不确定性, 即使程序员确保程序正确, 编译时依然会报错**

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
  
  public class TestExcep_2
  {
      public static void main(String[] args)
      {
          int m;
          try
          {
              m = 2;
          }
          catch (Exception e)
          {
              System.out.printf("...");
          }
  
          System.out.printf("m = %d", m);
      }
  }
  ------------------------------------------------------------------------------
  结果:
  java: 可能尚未初始化变量m
  ```

  

---

-  `Exception` 是所有`RuntimeException` 的父类, 因为 java 支持多态, 所以我们都可以全部写成 `Exception` 来捕获异常

