## Iterator接口

<img src="img/Iterator接口.png" alt="Iterator接口" style="zoom:60%;" />

#### Iterator方法介绍

<img src="img/Iterator方法介绍.png" alt="Iterator方法介绍" style="zoom:60%;" />



#### 遍历的实现方法

```java
vpackage a;

import java.util.Collection;
import java.util.Iterator;

public class TestIterator_1
{
    public static void showCollection(Collection c)
    {
        Iterator it = c.iterator();
        while (it.hasNext())
        {
            System.out.println(it.next());
        }
    }
}
```

