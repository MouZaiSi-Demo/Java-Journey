```java
package a;

public class TestInt
{
    public static void main(String[] args)
    {
        int i = 345;
        String str;

        //第一种方法
        /*str = i + "";
        System.out.println("str = " + str);*/

        /*//第二种方法
        Integer it = new Integer(i);
        str = it.toString();
        System.out.println("str = " + str);*/

        //第三种
        /*str = Integer.toString(i);
        System.out.println("str = " + str);*/

        //第四种
        /*str = String.valueOf(i);
        System.out.println("str = " + str);*/
    }
}

```

