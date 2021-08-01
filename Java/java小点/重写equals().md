#### 重写一个内容一致的情况下就返回true的equals()



- 第一个问题: 父类无法调用子类特殊的成员

```java
package a;

public class A
{
    public int i;

    public A(int i)
    {
        this.i = i;
    }

    public boolean equals(Object obj)
    {
        if(this.i == obj.i)
            return true;
        else
            return false;
    }
}
------------------------------------------------------------------------------
package a;

public class TestStringEquals_1 {
    public static void main(String[] args)
    {
        A aa1 = new A(2);
        A aa2 = new A(2);

        System.out.println(aa1.equals(aa2));
    }
}
------------------------------------------------------------------------------
结果:
java: 找不到符号
  符号:   变量 i
  位置: 类型为java.lang.Object的变量 obj
```

- 改进:

  ```java
  package a;
  
  public class A
  {
      public int i;
  
      public A(int i)
      {
          this.i = i;
      }
  
      public boolean equals(Object obj)
      {
          A aa = (A)obj;
          
          if(this.i == aa.i)
              return true;
          else
              return false;
      }
  }
  ------------------------------------------------------------------------------
  package a;
  
  public class TestStringEquals_1 {
      public static void main(String[] args)
      {
          A aa1 = new A(2);
          A aa2 = new A(2);
  
          System.out.println(aa1.equals(aa2));
      }
  }
  ------------------------------------------------------------------------------
  结果:
  true
  ```

  