## 如何理解面对对象的操作?

怎么从面向过程的思维转化成面对对象?

- 静态的属性 + 动态的操作 = 面对对象
- 重点在于先 **模拟事物**



---

在C语言中

面向过程一般都是静态的, 

```c
int Triangle_perimeter(int a, int b, int c)
{
	return a + b + c;
}
```

然后一个事物的属性也是静态的

```c
struct Student
{
	int id;
	char sex;
	float score;
};
// 其中, c语言中struct不允许有动态的东西在, 比如写个函数是不被允许的
```



java为了支持动态操作

把struct改进成class

```java
class Student
{
	int id;
	char sex;
	float score;
	
	void study()// java中的函数也叫做方法 方法逻辑意义代表的就是一个事物可以执行的操作
	{
	
	}
    void sleep()// 不需要定义形参, 因为id, sex, score与这个函数是一个有机整体,彼此可以直接访问 
    {
        
    }
}
```



然后class中的操作与C语言有对比

```java
class M
{
    public static void main(String[] args)
    {
    	//静态分配
		int i;
		//动态分配
		int *p = (int*)malloc(sizeof(int));
		//更一般的写法, 其中A是一个事物对象, 此时通过q就可以找到这个事物了
		A * q = (A *)malloc(sizeof(A));
        //java中, 本质上和C是一样的
        A s = new s();
        
        s.a = 3;
        s.b = 4;
        s.c = 5;
        
        System.out.printf("%d   %f\n", s.zhouchang(), s.area());
        //在java中, double和float都是 %f
        
    }
}
```

