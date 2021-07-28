## Java异常

#### 大纲

- 为什么需要异常
- 异常的处理机制
- 异常的处理
- 异常处理步骤
  - throw try catch finally throws
- 自定义异常
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

