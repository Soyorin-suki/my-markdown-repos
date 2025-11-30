# java学习
## 面向对象
### 面向对象基础
类(class)，实例(instance)，字段(field)
> 注意
> 一个java源文件可以包含多个类的定义，但只能定义一个public类，且public类名必须要和文件名相同，如果要定义多个public类，必须分拆到多个java源文件中

方法(method)

java的`this`不是指针，使用是`this.varible`而不是`this->varible`

可变参数`setName(String...name)`可变参数相当于数组类型，也可写成`setName(String[]name)`但是这样调用就需要`tmp.setName(new String[]("a","b","c"))`
但是使用可变参数则**需要注意可能会导致`tmp.setName(null)`**

**参数绑定**
调用方把参数传给实例方法时，调用时传递的值会按照参数位置一一绑定
基本类型参数的传递，是调用方值的复制，双方各自的后续修改，互不影响
引用类型参数的传递，调用方和传参方的参数变量指向的是同一个对象。双方任意一方对该对象的修改，都会影响对方

如果构造函数中没有初始化相关字段，则引用变量默认是null，数值类型则是默认值
java支持方法重载(overload)

#### 继承
与c++不同，java的继承是使用`extends`
OOP术语: 超类(super class)，父类(parent class)，基类(base class)，子类(subclass)，扩展类(extended class)

我们在定义一个类的时候，如果没有使用`extends`，则编译器会自动加上`extends Object`。所以任何类，除了`Objects`，都会继承于某个类

`private`和`public`和`protected`与c++中的一致

`super`关键词表示父类(超类)，子类引用父类的字段时可以使用`super.fieldName`
有些情况下使用`super.fieldName`和`this.fieldName`和`fieldName`是一样的，但是有些情况下必须使用`super`

在java中，任何`class`构造方法的第一句必须是父类的构造方法，如果没有明确的调用，编译器会自动添加一句**无参数**的父类构造函数`super()`，此时如果父类**没有**无参数的构造函数就会报错
因此如果父类没有默认的构造方法，子类就必须显式调用`super()`并给出参数
所以子类**不会继承**任何父类的构造方法，子类默认的构造方法是编译器自动生成的，不是继承的

**阻止继承**
如果一个类前拥有`final`则任何类都不能从这个类继承
从java15开始，允许使用`sealed`修饰这个类并使用`permits`关键字明确写出可以从该class继承的子类的名称
例如
```java
public sealed class Shape permits Rect, Circle, Triangle{
	...
}
```

**向上转型(upcasting)**
与c++类似，父类指针可以指向子类，但在java中则是使用`new`时可以用父类引用变量指向子类引用变量，比如：
若`class Student extends Person`则
```java
Person p=new Student();
```
**向下转型(downcasting)**
和向上转型相反，如果要讲一个父类类型强制转换为子类类型。
此时很有可能RE，子类的功能很有可能比父类多，多的功能无法实现
失败的时候Java虚拟机会报`ClassCastException`错误
为了避免向下转型错误，Java提供了`instanceof`操作符，用以判断一个实例是否是某个类，是的时候返回1，否则返回0
如果一个类是null，则会返回0

从Java14开始，判断`instanceof`时可以直接转型为指定变量，防止再次强制转型。例如：
```java
Object obj="hello";
if(obj instanceof String){
	String s=(String)obj;
	System.out.println(s.toUpperCase());
}
```
可以写成
```java
Object obj="hello";
if(obj instanceof String s){
	System.out.println(s.toUpperCase());
}
```
这种方法更加简洁

**继承和组合**
这不是我们c++里的has-a关系(组合)和is-a关系(继承)吗，怎么跑到java里来了

Java只允许单继承，所有类的最终的根类是Object；

#### 多态
覆写(override)
在继承关系中子类如果定义了一个与父类方法签名完全相同的方法，称为覆写
`Override`与`Overload`不同点在于如果方法签名不同，则是`Overload`，如果方法签名相同，且返回值相同则是`Override`
> 方法名相同，方法参数相同，但方法返回值不同，也是不同的方法，在Java中，出现这种情况会报错

加上`@Override`可以让编译器检查是否进行了正确的覆写。希望进行覆写，但方法签名写错的话编译器会报错
但是`@Override`不是必须的

多态(Polymorphic)
Java的实例方法调用时是基于运行时的实际类型的动态调用，而非变量的声明类型。这个特性被称为多态
利用多态可以实现更多类型的子类实现功能拓展而不需要改变基于父类的代码

在子类的覆写方法中，如果想要调用父类未修改的方法，可以使用`super`来调用
可以通过将一个方法修饰为`final`来防止方法被覆写
被`final`修饰的类不能被继承
被`final`修饰的字段不能在初始化之外被重新赋值，在构造函数中可以对字段初始化

#### 抽象类
如果一个`class`定义了方法，但没有具体执行代码，这个方法就是**抽象方法**，抽象方法用`abstract`修饰。因为无法执行抽象方法，因此这个类也必须要声明为`abstract class`抽象类，抽象类无法被实例化，只能被继承

**面对抽象编程**
当我们定义了抽象类`Person`和具体的子类`Teacher`和`Student`时，可以通过抽象类去引用具体子类的实例：
```java
Person t=new Teacher();
Person s=new Student();
```
这种引用抽象类的好处是：我们队方法进行调用时，并不用关心`Person`类型变量的具体子类型
这样尽量引用高层类型，避免引用实际子类型的方式，称为**面向抽象编程**
面向抽象编程的本质是：
- 上层代码只定义规范
- 不需要子类就可以实现业务逻辑
- 具体的业务逻辑由不同的子类实现，调用者并不关心

#### 接口
在抽象类中，抽象方法的本质上是定义接口规范：即规定高层类的接口，从而保证所有子类都有相同的接口实现，这样，多态才可以发挥威力
如果一个抽象类没有字段，且所有方法都是抽象方法，那么就可以将这个抽象类改写为接口`interface`
```java
interface Person{
	void fun();
	String getName();
}
```
`interface`连字段都不能有，所有接口定义的方法默认都是`public abstract`的，所以写不写都一样
如果一个具体的`class`去实现一个`interface`时，需要使用`implements`而不是`extends`
在Java中，一个类只能继承一个类，但是一个类可以实现多个接口
例如：
```java
class Student implements Person,Hello{
	...
}
```
抽象类与接口的对比：
||abstract class|interface|
|---|---|---|
|继承|只能extends一个class|可以implements多个interface|
|字段|可以定义实例字段|不能定义实例字段|
|抽象方法|可以定义抽象方法|可以定义抽象方法|
|非抽象方法|可以定义非抽象方法|不能定义非抽象方法|

**接口继承**
一个`interface`可以继承另一个`interface`，使用`extends`

**default方法**
在`interface`中，可以使用`default`修饰方法
`default`方法的目的是，当我们需要给接口新增一个方法时，需要修改全部子类，但如果使用`default`方法，子类就只需要在对应的地方修改需要修改的地方即可
例如：
```java
interface run{
	String getName();
	void run(){
		System.out.println(getName()+" run");
	}
}
```
#### 静态字段和静态方法
与c++类似
通常情况下，使用实例变量访问静态方法只会得到一个编译警告，编译器会自动将其修改为类名
**接口的静态字段**
因为`interface`是一个纯抽象类，所以不能定义实例字段，但是可以定义静态字段，并且字段必须是`final`类型
实际上，因为`interface`的字段只能是`public static final`类型的，所以可以省略这些，编译器会自动将其转换为`public static final`类型

#### 包
与c++的命名空间类似，Java解决命名冲突使用的是**包**
包`package`是一种命名空间，一个类总属于某个包，真正完整的类名是`包名.类名`
在定义`class`时，我们需要在第一行声明`class`是哪个包
在java虚拟机执行的时候，JVM只看完整类名，因此只要包名不同，类就不同
包可以是多层结构，用`.`隔开，例如`java.util`
包没有父子关系，`java.util`和`java.util.zip`是不同的包，两者没有继承关系
没有包名的类使用的是默认包，容易出现命名冲突
我们还需要按照包结构将上面的Java文件组织起来，假设以`package_sample`为根目录，那么文件结构就是
package_sample/src/{hong/Person.java}{ming/Person.java}{mr/jun/Arrays.java}
类似的编译后的`.class`文件也要以相似的结构组织
package_sample/bin/{hong/Person.class}{ming/Person.class}{mr/jun/Arrays.class}

**包作用域**
不用`public`,`private`,`protected`修饰的字段和方法就是包作用域
位于同一个包的类，可以访问包作用域的字段和方法

**import**
使用`import`引入一个包或一个包的类
`import java.util`之后可以使用`java.util.Arrays`
使用`import java.util.Arrays`之后可以直接使用`Arrays`
可以使用`import java.util.*`把这个包(不包含子包)的所有类引入进来
还有一种`import static java.lang.System.*`可以引入一个类的静态字段或方法

java编译器最终编译出来的`.class`文件只使用**完整类名**。因此，在代码中，当编译器遇到一个`class`类名时：
- 如果是完整类名，就直接根据完整类名查找这个`class`
- 如果是简单类名
  - 查找当前`package`是否存在这个`class`
  - 查找`import`的包里有没有这个`class`
  - 查找`java.lang`的包里有没有这个`class`

如果按照上述规则仍然无法确定类名，则编译报错
在编写class时编译器会自动`import`当前`package`里的其他所有`class`和`java.lang.*`
> `java.lang`包与`java.lang.reflect`包不是同一个包，后者仍需手动导入

如果有两个类的简单类名相同，那只能`import`一个，另一个必须写完整类名

一般为了避免出现包名冲突，推荐使用倒置的域名来保持唯一性，同时避免与`String`,`System`,`Runtime`等常用类重名

**编译与运行**
例如work/{bin}{src/com/itranswarp/{sample/Main.java}{world/Person.java}}
首先确定是work文件夹
```
$ ls
bin src
```
然后编译`src`目录下所有Java文件
```
$ javac -d ./bin src/**/*.java
```
命令行`-d`指定输出的`.class`文件存放处，后面的表示编译所有`src`目录下的`.java`文件，包括任意深度的
Windows不支持`**`这种搜索全部子目录的做法，所以Windows下要一次列出所有的`.java`文件

#### 作用域

**public**
定义为`public`的`class`,`interface`可以被其他任何类访问
**private**
定义为`private`的`field`,`method`不能被其他类访问，即没限定在了该`private class`内部
由于java支持嵌套类，所以嵌套类也可以访问`private`
嵌套类(nested class)
**protected**
`protected`用于继承关系，定义为`protected`的字段和方法可以被子类以及子类的子类访问
**package**
包作用域是指一个类允许访问同一个`package`里的没有`public`,`private`修饰的`class`,以及没有`private`,`protected`修饰的字段和方法
**局部变量**
和c++一样
**final**
与c++中的const类似，但是用在`class`上可以防止`class`被`extends`

#### 内部类
一个类被定义在一个类的内部时，被称为内部类(嵌套类)(nested class)
**Inner class**
如果一个类被定义在一个类的内部，这个类就是`Inner Class`
```java
class Outer{
	class Inner{

	}
}
```
`Inner Class`最大的特点在于它不能单独存在，必须依附于一个`Outer`实例
```java
public class Main{
	public static void main(String[] argc){
		Outer outer=new Outer("Nested");
		Outer.Inner inner=outer.new Inner();
		inner.hello();
	}
}
class Outer{
	private String name;
	Outer(String name){
		this.name=name;
	}
	class Inner{
		void hello(){
			System.out.println("Hello\n");
		}
	}
}
```
`Inner Class`可以修改`Outer class`的`private`字段
观察Java编译器编译出来的`.class`文件可以发现`Outer`类被编译成`Outer.class`而`Inner`类被编译成`Outer$Inner.class`
**Anonymous Class**
匿名类(Anonymous Class)是不在类中显式定义类，而是在方法内部实例化一个`Runnable`,如下：
```java
public class Main{
	public static void main(String[] args){
		Outer outer=new Outer("Nested");

	}
}
class Outer{
	private String name;
	Outer(String name){
		this.name=name;
	}
	public void hello(){
		Runnable r=new Runnable(){
			@Override
			public void run(){
				System.out.println("hello");
			}
		}
		new Thread(r).start();
	}
}
```
`Runnable`是接口，接口是不能实例化，所以实际上是定义了一个实现了`Runnable`接口的匿名类，写法如下：
```java
Runnable r=new Runnable(){
	//必要的抽象方法
};
```
观察Java编译器编译出来的`.class`文件可以发现`Outer`类被编译成`Outer.class`而匿名类被依次编译成`Outer$1.class`,`Outer$2.class`
除了接口外，匿名类也可以继承自普通类
**Static Nested Class**
静态内部类(Static Nested Class)
顾名思义

#### classpath和jar
**classpath**
什么是`classpath`？
`classpath`就是JVM用到的一个系统变量，用来指示JVM如何搜索`class`
因为JAVA是编译型语言，源码文件是`.java`，而编译后的`.class`文件才是真正的可以被JVM执行的字节码，因此，JAVA需要知道如果要加载一个`abc.xyz.Hello`的类，需要去哪里搜索对应的`Hello.class`文件
所以，`classpath`就是一组目录的集合，它设置的搜索路径与操作系统相关。
例如，在Windows系统上，用`;`隔开，带空格的目录用`""`括起来：
`C:\work\project1\bin;C:\shared;"D:\My Document\project1\bin"`
在Linux系统上，用`:`分割

现在假设`classpath`是`.;C:\work\project1\bin;C:\shared`，当JVM在加载`abc.xyz.Hello`时，会依次查找：
- <当前目录>\abc\xyz\Hello.class
- C:\work\project1\bin\abc\xyz\Hello.class
- C:\shared\abc\xyz\Hello.class
> `.`代表当前目录

如果JVM在某个路径下搜索到了`Hello.class`文件，则不再继续往后搜索，如果所有路径都没有找到，就报错

`classpath`的设定方法有两种：
在系统变量中设置`classpath`，**不推荐**，因为会污染整个系统环境；
在启动JVM时设置`classpath`，推荐。

第二个方法实际上就是给`java`命令传入`-classpath`参数(也可以使用`-cp`)：
`java -cp .;C:\work\project1\bin abc.xyz.Hello`
如果没有设置`-cp`参数，则默认使用`.`即当前文件夹
> 在IDE中运行java程序，IDE自动传入的`-cp`参数是当前工程的`bin`目录和引入的jar包

> 注意不要把任何java核心库放入环境变量中，JVM不依赖任何`classpath`加载核心库

更好的做法是不使用`classpath`,默认的`.`对于绝大多数情况都够用了

**jar包**
如果有很多`.class`文件散落在各层目录当中，肯定不便管理，如果能将目录打包成一个包，变成一个文件就好了
jar包就是用来干这个事儿的，它将`package`组织的目录层级，以及各个目录下的所有文件(包括`.class`和其他文件)都打包成一个jar文件，这样一来，无论是备份还是发给客户都方便得多
jar包实际上就是一个zip压缩格式的压缩包，而jar包相当于目录，如果要执行一个jar包的`.class`，就可以将jar包放到`classpath`中：
`java -cp ./hello.jar abc.xyz.Hello.class`

创建jar包：因为jar包就是zip包，所以直接将其打包成`.zip`然后改成`.jar`即可
> 注意，jar包里的第一层目录不能是`bin`
> 因为`abc.xyz.Hello.class`必须按照`abc\xyz\Hello.class`而不是`bin\abc\xyz\Hello.class`

jar包还可以包含一个特殊的`/META-INF/MANIFEST.MF`文件，`MANIFEST.MF`是纯文本，可以指定`Main-class`等和其他信息。JVM会自动读取这个文件，如果存在`Main-class`我们就不必指定启动的类名，而是用更方便的命令：
`java -jar hello.jar`
在大型项目中，不可能手动编写`MANIFEST.MF`文件，再手动创建jar包。java社区提供了大量的开源工具，例如Maven，可以方便的创建jar包

#### class版本

通常说的java 8，java 11等都说的JDK的版本，也就说JVM的版本，更确切的说是`java.exe`的版本
``
$ java -version
``
可以查看版本
而每个版本的JVM能执行的class文件版本也不同。例如Java 11对应的class版本是55，而Java 17对应的class版本是61
用对应的Java编译输出的class版本默认是对应的class版本
一个61的class文件可以在Java17、Java18上运行，但是不能再Java11上运行，会抛出`UnsupportClassVersionError`
可以指定编译输出版本，一种是`javac`命令行中用参数`--release`设置：
`javac --release 11 Main.java`表示输出class版本兼容Java11，即55版本

第二种是用参数`--source`指定源码版本，`--target`指定输出class版本：
`javac --source 9 --release 11 Main.java`将源码视为Java9兼容版本，并编译输出兼容Java11的class版本
注意`--release`和`--source --target`只能二选一

使用后者时可能会出现方法在当前JDK中存在但是在release版本中不存在的情况
使用前者则会子啊编译时检查各种方法在release版本中是否存在
**源码版本**

#### 模块
从Java 9开始，引入了模块(Module)


