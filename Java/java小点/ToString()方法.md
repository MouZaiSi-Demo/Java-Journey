## toString方法总结

- 所有的类都默认自动继承了Object类

- Object类中的 toString方法返回的是类的名字和该对象哈希码组成的一个字符串

- **System. out.println(类对象名);**

  **实际输出的是该对象的 tostring（）方法所返回的字符串, 也就是会调用一次toString**

- 为了实际需要，建议子类重写从父类 Object:继承的 tostring方法

