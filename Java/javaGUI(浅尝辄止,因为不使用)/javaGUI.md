## GUI

---

#### 组件

- 组件( Component)是图形用户界面的基本组成元素，凡是能够以图形化方式显示在屏幕上并能够与用户进行交互的对象均为组件，如菜单、按钮、标签、文本框、滚动条等
- 组件分类
  - `java. awt. Component`
  - `Java.awt. Menucomponent`
  - 说明：抽象类`java.awt. Component`是除菜单相关组件之外所有`JavaAWT`组件类的根父类，该类规定了`GUI`组件的基本特性，如尺寸、位置和颜色效果等，并实现了作为一个`GUI`部件所应具备的基本功能



---

#### 容器

- 组件通常不能独立地显示出来，必须将组件放在一定的容器中才可以显示出来
- 有一类特殊的组件是专门用来包含其他组件的，这类组件叫做容器，`java.awt. Container`是所有容器的父类，`java.awt. Container`继承自`java.awt. Component`
- 容器类对象本身也是一个组件，具有组件的所有性质，但反过来组件却不一定是容器

---

#### Panel

- panel是容纳其他组件的组件
- panel是容器
- pane木能单独存在，必须得被添加到其他容器中
- Panela类拥有从其父类继承来的
  - setBounds(int x, int y, int width, int height)
  - setSize(int width, int height)
  - setLocation(int x, int y)
  - setBackground( Color c)
  - setayout( LayoutManager mgr)等方法。
- Panell的构造方法为：
  - Panel() 使用默认的 Flowlayout:类布局管理器初始化。
  - Panell( LayoutManager layout) 使用指定的布局管理器初始化。

---

#### 布局管理器

- 容器对其中所包含组件的排列方式，包括组件的位置和大小设定，被称为容器的布局( Layout).
- 为了使图形用户界面具有良好的平台无关性，Java语言提供了布局管理器来管理容器的布局，而<strong style="color:red;">不建议直接设置组件在容器中的位置和尺寸</strong>。
- 每个容器都有一个默认的布局管理器，当容器需要对某个组件进行定位或判断其大小尺寸时，就会自动调用其对应的布局管理器。
- 在AWT中，常见的布局管理器: 
  - BorderLayout
  - FlowLayout
  - GridLayout