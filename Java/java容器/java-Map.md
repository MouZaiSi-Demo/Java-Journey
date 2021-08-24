## Map

本质就是数据结构的哈希表



---

#### 代码例子

```java
package A;

import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

public class TestMap
{
    public static void main(String[] args)
    {
        Map<String, Integer> m = new HashMap<String, Integer>();
        m.put("1", 1);
        m.put("2", 2);
        m.put("3", 3);
        System.out.println(m.get("3"));
    }
}
```



#### 遍历Map所有元素的value

```java
Set s = m.keySet();
Iterator it = s.iterator();
while (it.hasNext())
{
    String key= (String) it.next();// 这里因为it.next()会返回父类的, 然后便于你实现多态, 因为事先他不知道你要返回什么类型
    System.out.println(m.get(key));
}
```

