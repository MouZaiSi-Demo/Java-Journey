## Object类的equals方法

- 所有类都从 Object 类中继承了 equlas方法

- Object 类中equals方法源码如下

  ```java
  public boolean equals(Object obj)
  {
  	return this == obj;
  }
  ```

- object中的 equals方法是直接判断 this 和 obj 本身的值是否相等，即用来判断调用 equals 的对象和形参 obj 所引用的对象是否是同一对象，**所谓同一对象就是指是内存中同一块存储单元**，如果 this 和 obj 指向的是同块内存对象，则返回true,如果 this 和 obj 指向的不是同一块内存，则返回 false,注意：即使是内容完全相等的两块不同的内存对象，也会返回 false

- 如果是同一块内存，则 object中的 equals方法返回true,
  如果是不同的内存，则返回 false



---

#### 何时需要重写?

- 用一个类构造出来的不同内存的两个对象，如果内存中的值相等，我们一般情况下也应该认为这两个对象相等，很明显Objecti中的 equals(无法完成这样的重任， Objectl中的equals(方法只有在两个对象是同一块内存时，才返回true,这时候我们就有必要重写父类 Object中的 equals方法
- 如果希望不同内存但是内容相同的两个对象equals时返回true, 则我们需要重写父类的equals方法