## StringBuffer类由来

- String类对象一旦创建就不可更改

- 如果经常对字符串内容进行修改，则使用 StringBuffer.

- <strong style="color:red;">如果经常对字符串内容进行修改而使用 String的话，就会导致即耗空间又耗时间！</strong>

- 例子

  - String s1= “ abasdmas";             String str2 = “123"; 

  - 1. String s1 = s1 + s2;
  - 2. 删除str1中的字母d

- StringBuffer 对象的内容是可以改变的

- <strong style="color:red;">因此 String类中没有修改字符串的方法，但是StringBuffer类中却有大量修改字符串的方法</strong>



---

#### StringBuffer类的构造函数

- `public StringBuffer()`
  - 创建一个空的没有任何字符的 Stringbuffer对象
- `public Stringbuffer(int capacity)`
  - 创建一个不带字符，但具有指定初始容量的字符串缓冲区
- `public Stringbuffer(String str)`
  - 创建一个 String>对象，包含与str对象相同的字符序列