#### 生产消费

---

**问题:**

- 一个仓库最多容纳6个产品，制造商现在要制造20件产品存入仓库，消费者要从仓库取出这20件产品来消费
- 制造商制造产品和消费者取出产品的速度很可能是不一样的, 编程实现两鼇的同步

---

需要用到的知识:

栈

线程同步

---

栈是用来维护生产消费的操作, 线程保证维护的时候的正确性

---

##### 需要注意的地方

- 执行完20行的代码后，程序绝对不会立即切换到另一个线程
- 叫醒的是其他线程，叫醒的不是本线程
- 在最开始，P和C刚开始执行时
  - 即便P没有`wait()`也可以在`C`中 
  - 即便C没有 `wait`,也可以在`P`中 `notify()`

---

#### notify 和 wait方法提示(代码下面有总结)

`aa.notify()`, 的时候即使没有任何线程处于`wait()`, 也就时阻塞状态都是可以使用的 

---



```java
import java.beans.Customizer;

public class TestPC
{
    public static void main(String[] args)
    {
        SynStack ss = new SynStack();
        Producer p = new Producer(ss);// 让p操作ss
        Consumer c = new Consumer(ss);// 让c操作ss

        //生产和消费的线程的启动
        Thread t1 = new Thread(p);
        t1.start();
        Thread t2 = new Thread(c);
        t2.start();
    }
}
------------------------------------------------------------------------------
public class SynStack
{
    private char[] data = new char[6];
    private int cnt = 0;// 表示数组的有效性
    private int idx = 1;

    public synchronized void push(char ch)
    {
        while(cnt == data.length)// 当栈满的时候就会停止
        {
            try
            {
                this.wait();
            }
            catch (Exception e)
            {

            }
        }
        this.notify();

        data[cnt ++] = ch;
        System.out.printf("生产线程正在生产第%d个产品, 该产品是: %c\n", cnt, ch);

    }

    public synchronized char pop()
    {
        char ch;

        while(cnt == 0)// 当没有的时候会停止该线程, 此时才唤醒生产线程
        {
            try
            {
                this.wait();
            }
            catch (Exception e)
            {
            }
            this.notify();
        }
        if(cnt == data.length) this.notify();// 该条是防止当满了后, push被阻塞, 唤醒pop, 但是push就不会再被唤醒的bug
        ch = data[cnt - 1];
        System.out.printf("消费线程正在下消费第%d个产品, 该产品是: %c\n", cnt, ch);
        cnt --;
        return ch;
    }
}
------------------------------------------------------------------------------
public class Producer implements Runnable
{
    // 接收操作的对象, 这里是ss
    private SynStack ss = null;
    public Producer(SynStack ss)
    {
        this.ss = ss;
    }

    public void run()
    {
        char ch;
        for(int i = 0; i < 20; i ++)
        {
            try
            {
                Thread.sleep(100);
            }
            catch (Exception e)
            {

            }
            ch = (char)('a' + i);
            ss.push(ch);
        }
    }
}
------------------------------------------------------------------------------
public class Consumer implements Runnable
{
    // 接收操作的对象, 这里是ss
    private SynStack ss = null;
    public Consumer(SynStack ss)
    {
        this.ss = ss;
    }

    public void run()
    {
        for(int i = 0; i < 20; i ++)
        {
            try
            {
                Thread.sleep(200);
            }
            catch (Exception e)
            {

            }
            ss.pop();
        }
    }
}
```



---

#### notify 和  wait方法总结

- `aa. wait()`
  - 将执行`aa.wait()`的当前线程转入阻塞状态，让出CPU的控制权
  - <strong style="color:red;">释放对`aa`的锁定</strong>
- `aa notify()`
  - 假设执行 `aa. notify()` 的当前线程为T1
  - 如果当前时刻有其他线程因为执行了 `a.wait() `而陷入阻塞状态，则叫醒其中的一个
  - 所谓叫醒某个线程就是令该线程从因为 `wait()` 而陷入阻塞的状态转入就绪状态
- `aa. notifyall`
  - 叫醒其他<strong style="color:red;">所有的</strong>因为执行了 `aa.wait()` 而陷入阻塞状态的线程



---

