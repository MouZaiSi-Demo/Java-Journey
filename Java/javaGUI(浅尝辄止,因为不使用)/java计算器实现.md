## 利用java的awt的GUI来实现计算器



主要是理解跨类使用属性的操作

因为GUI并没有去学, 所以GUI的很多操作是抄的

---

#### 方法1

```java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class MyMonitor implements ActionListener
{
    @Override
    public void actionPerformed(ActionEvent e)
    {
        int num1 = Integer.parseInt(TestTextField_1.tf1.getText());
        int num2 = Integer.parseInt(TestTextField_1.tf2.getText());
        int num3 = num1 + num2;
        TestTextField_1.tf3.setText(num3 + "");
    }
}
------------------------------------------------------------------------------
import java.awt.*;

public class TestTextField_1
{
    public static TextField tf1, tf2, tf3;
    public static void main(String[] args)
    {
        Frame f = new Frame();
        tf1 = new TextField(10);
        tf2 = new TextField(10);
        tf3 = new TextField(10);
        Label Lb = new Label("+");
        Button bn = new Button("=");
        f.setLayout(new FlowLayout());
        f.add(tf1);
        f.add(Lb);
        f.add(bn);
        f.add(tf2);
        f.add(tf3);

        bn.addActionListener(new MyMonitor());

        f.pack();
        f.setVisible(true);
    }
}
```



#### 方法2(类似生产消费的传递this的方式)

```java
import java.awt.*;

public class TF
{
    public TextField tf1, tf2, tf3;

    public void launch()
    {
        Frame f = new Frame();
        tf1 = new TextField(10);
        tf2 = new TextField(10);
        tf3 = new TextField(10);
        Label Lb = new Label("+");
        Button bn = new Button("=");
        f.setLayout(new FlowLayout());
        f.add(tf1);
        f.add(Lb);
        f.add(bn);
        f.add(tf2);
        f.add(tf3);

        bn.addActionListener(new MyMonitor(this));

        f.pack();
        f.setVisible(true);
    }
}
------------------------------------------------------------------------------
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowEvent;
import java.awt.event.WindowFocusListener;

public class MyMonitor implements ActionListener
{
    private TF tf = null;

    public MyMonitor(TF tf)
    {
        this.tf = tf;
    }

    @Override
    public void actionPerformed(ActionEvent e)
    {
        int num1 = Integer.parseInt(tf.tf1.getText());
        int num2 = Integer.parseInt(tf.tf2.getText());
        int num3 = num1 + num2;
        tf.tf3.setText(num3 + "");
    }
}
------------------------------------------------------------------------------
import java.awt.*;
import java.awt.event.WindowFocusListener;

public class TestTextField_1
{
    public static void main(String[] args)
    {
        new TF().launch();
    }
}
```



#### 方法3(内部类, 最优方法)

```java
import java.awt.*;
import java.awt.event.WindowFocusListener;

public class TestTextField_1
{
    public static void main(String[] args)
    {
        new TF().launch();
    }
}
------------------------------------------------------------------------------	
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class TF
{
    private TextField tf1, tf2, tf3;

    public void launch()
    {
        Frame f = new Frame();
        tf1 = new TextField(10);
        tf2 = new TextField(10);
        tf3 = new TextField(10);
        Label Lb = new Label("+");
        Button bn = new Button("=");
        f.setLayout(new FlowLayout());
        f.add(tf1);
        f.add(Lb);
        f.add(bn);
        f.add(tf2);
        f.add(tf3);

        bn.addActionListener(new MyMonitor());

        f.pack();
        f.setVisible(true);
    }
    public class MyMonitor implements ActionListener
    {
        @Override
        public void actionPerformed(ActionEvent e)
        {
            int num1 = Integer.parseInt(tf1.getText());
            int num2 = Integer.parseInt(tf2.getText());
            int num3 = num1 + num2;
            tf3.setText(num3 + "");
        }
    }

}
```

