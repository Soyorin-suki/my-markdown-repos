# Swing
## 概述
Swing 组件通常被称为“轻量级组件”，它完全由Java编写，不依靠操作系统
类继承关系为：
`Java.long.Object`->`Java.awt.Componet`->`Java.awt.Container`->`Javax.swing.JComponet`
awt包是抽象窗口工具包(Abstract Window Toolkit)
JComponet 是 swing 组件存放的位置
常用组件如下：
|||
|-|-|
|JFrame|框架|
|JDialog|对话框|
|JOptionPane|对话框|
|JButton|按钮|
|JCheckBox|复选框|
|JConbox|下拉框|
|JLable|标签|
|JRadioButton|单选按钮|
|JList|显示一组条目的组件|
|JTextField|文本框|
|JPasswordField|密码框|
|JTextArea|文本区域|

JavaSwing组件之间的继承关系为；
componet->Container->{Jcomponet->{JPanel}{JTable}{JTextArea}{JTextField}{JButton}}{Window->{Frame->JFrame}{Dialog->JDialog}}

## JFrame
JFrame是一个容器，它是各个组件的载体。开发过程中，通过继承`java.awt.JFrame`来创建
### 新建JFrame对象
```java
JFrame()//创建没有标题的窗口
JFrame(String s)//创建标题为s的窗口
```
### 设置JFrame大小
```java
public void setSize(int width,int height)//设置窗口大小
public void setLocation(int x,int y)//设置窗口位置，默认(0,0)
public void setBounds(int x,int y,int width,int height)//设置窗口位置为(x,y)，和窗体大小
public void setVisible(boolean b)//设置窗口是否可见，默认不可见
public void setResizable(boolean b)//设置窗口能否调整大小，默认可调整大小
public void dispose()//撤销当前窗口并释放当前窗口使用资源
public void setExtendedState(int state)//设置窗口的扩展状态，state可取JFrame类中的以下常量
MAXIMIZED_HORIZ//水平方向最大化
MAXIMIZED_VERT//竖直方向最大化
MAXIMIZED_BOTH//两个方向都最大化
```
### 设定JFrame的关闭方式
```java
public void setDefaultCloseOperation(int operation)//该方法设置单机关闭窗口后，程序做出何种处理，其中的参数取JFrame类中的以下static常量：
DO_NOTHING_ON_CLOSE//什么也不做
HIDE_ON_CLODE//隐藏当前窗口
DISPOSE_ON_CLOSE//同上，并释放资源
EXIT_ON_CLOSE//结束程序
```
### 设定JFrame的布局
```java
.setLayout(new FlowLayout());
// 或GridLayout
```

## JDialog
继承自java.awt.Dialog类，从一个窗体弹出的另外一个窗体，它和JFrame类似，但是需要调用getContentPane将窗体转换为容器。然后在容器中设置窗体的内容
```java
JDialog //可当成一JFrame使用，但必须从属于JFrame
JDialog();
JDialog(Frame f);//指定父窗口
JDialog(Frame f,String title);//指定父窗口+标题
```

## 常用的面板
面板也是一种swing容器，它可以作为容器添加容纳其他的组件，但是它自己必须被加在一个容器内
### JPanel
JPanel是一种最简单的面板，它继承自java.awt.Container类
e.g.:
```java
JFrame jf=new JFrame("test");
JFrame jp=new JPanel(new FlowLayout());
JButton jb=new JButton("1");
jp.add(jb);
jf.add(jp);
jf.setVisible(true);
```

### JScrollPane
带滚动条的JPanel

## 组件

### 标签组件
在JLabel类中，显示文本
构造函数：
```java
JLable();
JLable(Icon icon)//设置图标
JLable(Icon icon,int aligment)//设置图标和水平对齐方式
JLable(String str,int aligment)//设置文本和水平对齐方式
JLable(String str,Icon icon,int aligment)//以上
```
其中的`aligment`可以设置`SwingConstants.RIGHT`,`SwingConstants.CENTER`,`SwingConstants.LEFT`等

### 按钮组件
在JButton类中
构造函数：
```java
JButton();
JButton(String s)//指定文字
JButton(Icon icon)//指定图标
JButton(String s,Icon icon)//
```
其他方法：
```java
setTooltipText(String s)//
setBoarderPaint(boolean b)//设置边界是否显示
setEnabled(boolean b)//设置按钮是否可用
```

### 单选按钮组件
在JRadioButton类和ButtonGroup类中
是JRadioButton是一个单选按钮，需要将单选按钮加入到按钮组中
构造方法：
```java
JRadioButton();
JRadioButton(Icon icon);
JRadioButton(Icon icon,boolean selected);// 是否被选中
JRadioButton(String text);
JRadioButton(String text,Icon icon);
JRadioButton(String text,Icon icon,boolean selected);
ButtonGroup();
```

### 复选组件框
在JCheckBox类中
和上面类似
构造方法：
```java
JCheckBox();
JCheckBox(String text,boolean selected);
JCheckBox(Icon icon,boolean seleceted);
```

### 下拉列表组件
在JCombaBox类中
构造方法：
```java
JCombaBox();
JCombaBox(CombaBoxModel dataModel);//
JCombaBox(Object[] arrayData);//
JCombaBox(Vector vector);//
// 方法
.addItem();//添加下拉内容
```

### 菜单栏组件
一级菜单的：
首先创建菜单条，之后创建菜单，最后创建菜单项
```java
JMenuBar()//菜单条
JMenu()//菜单
JMenuItem()//菜单项
//菜单项依赖于菜单，菜单依赖于菜单条
```





