## String类的equals方法,==号,常量字符串



- 示例1:

  ```java
  package a;
  
  public class TestString_1
  {
      public static void main(String[] args)
      {
          // new一个对象会从heap那返回空间地址给stack区变量
          // 此时str1和str2分别指向两块不同的存有
          String str1 = new String("china");
          String str2 = new String("china");
          System.out.println(str1.equals(str2));
  
          if(str1 == str2)
              System.out.println("str1 == str2");
          else
              System.out.println("str1 != str2");
  		
          // 这里直接把显式的String常量赋给了stack区变量, 显式的String常量放在常量池
          // 此时str3和str4同时指向常量池里的"china"(常量池不拷贝)
          String str3 = "china";
          String str4 = "china";
          System.out.println(str3.equals(str4));
  
          if(str3 == str4)
              System.out.println("str3 == str4");
          else
              System.out.println("str3 != str4");
      }
  
  }
  ------------------------------------------------------------------------------
  结果:
  true
  str1 != str2
  true
  str3 == str4
  ```

  

