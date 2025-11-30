# IO
## File对象
Java的标准库`java.io`提供了`File`对象来操作文件和目录
要构造一个`File`对象，需要传入文件路径
构造时可以传入绝对路径也可以传入相对路径，例如：
```java
File file1=new File("C:\\code\\Java");
File file2=new File("Java");
File file3=new File(".\\Java")// .表示当前路径
File file4=new File("..\\code\\Java")// ..表示上级目录
```
`File`对象返回路径有三种方法：
- `getpath()`返回构造方法传入的路径
- `getAbsolutePath()`返回绝对路径
- `getCanoicalPath()`返回规范路径

### 文件与目录
`File`对象既可以表示一个文件也可以表示一个目录，构造`File`对象时即是文件或目录不存在也不会出错，只有当我们通过`File`对象进行磁盘操作（调用某些方法）时才会报错
我们可以通过调用`isFile()`方法判断该对象是否是一个文件，调用`isDirectory()`方法判断噶对象是否是一个目录
还可以进一步判断文件的权限和大小：
- `boolean canRead()`是否可读
- `boolean canWrite()`是否可写
- `boolean canExecute()`是否可执行
- `long length()`文件字节大小

### 创建与删除文件
当`File`对象表示一个文件时，可以通过`createNewFile()`创建一个新文件，用`delete()`删除一个文件
当需要使用临时文件时可以使用`createTempFile()`创建一个临时文件，该临时文件将会在JVM退出时自动删除
### 遍历文件与目录
当`File`对象表示一个目录时，可以使用`list()`和`listFiles()`列出目录下的文件和子目录名。`listFiles()`可以根据自己的需要过滤不想要的文件和目录
和文件操作类似，如果`File`对象表示一个目录的话可以通过以下方法创建和删除目录：
- `boolean mkdir()`创建当前File对象表示的目录
- `boolean mkdirs()`创建当前File对象表示的目录，并在必要时将父目录也创建出来
- `boolean delete()`删除当前File对象表示的目录，且目录为空时才可删除成功

### Path对象
`java.nio.file`中还有`Path`对象，如果需要复杂的路径操作使用`Path`对象会更加方便


## InputStream
`InputStream`是Java标准库提供的最基本的输入流，位于`java.io`包里，`java.io`包里提供了所有同步IO的功能
需要注意的是`InputStream`不是一个接口，而是一个抽象类，它是所有输入流的超类，这个抽象类定义的一个最重要的方法就是`int read()`：
```java
public abstract int read()throws IOException;
```
这个方法会读取输入流的下一个字节，并返回字节表示的`int`值，如果已读到末尾会返回`-1`
`FileInputStream`是`InputStream`的一个子类
下面是使用`FileInputStream`读取一个`FileInputStream`所有字节的实例：
```java
public void readFile()throws IOException{
	InputStream input=new FileInputStream("C:\\code\\java\\exp.txt");
	while(true){
		int n=input.read();
		if(n==-1)break;
		System.out.print(n);
	}
	input.close();
}
```
最后要把该流关掉，节约内存
仔细观察上述代码可以发现，如果发生了IO错误，就会导致该流没有关闭
因此，我们需要使用`try...finally`来保证最后输入流关闭：
```java
public void readFile()throws IOException{
	try{
    	InputStream input=new FileInputStream("C:\\code\\java\\exp.txt");
    	while(true){
    		int n=input.read();
    		if(n==-1)break;
    		System.out.print(n);
    	}
	}finally{
		if(input!=null)input.close();
	}
}
```
也可以使用java 7引入的新语法：`try(resource)`
```java
public void readFile()throws IOException{
	try(InputStream input=new FileInputStream("C:\\code\\java\\exp.txt")){
    	int n;
		while((n=input.read())!=-1){
    		System.out.print(n);
    	}
	}
}
```
这种写法的本质是编译器查看`try(resource = ...)`中的对象是否实现了`java.lang.AutoClosable`接口，如果实现了就会在最后加入`finally`语句并调用`close()`方法。`InputStream`和`OutputStream`都实现了这个接口
### 缓冲
与c++一样
`InputStream`提供了两个重载方法：
- `int read(byte[] b)`读取若干字节并填充到`byte[]`数组中，返回读取的字节数
- `int read(byte[] b,int off,int len)`指定`byte[]`数组的偏移量和最大填充数

如果返回`-1`代表没有更多数据了
利用缓冲区读取的代码如下：
```java
public void readFile()throws IOException{
	try(InputStream input=new FileInputStream("C:\\code\\java\\exp.txt")){
		byte[] buffer=new byte[1024];
		int n;
		while((n=input.read(buffer))!=-1){
			System.out.println("read "+n+" bytes.");
		}
	}
}
```

### 阻塞
在调用`InputStream`的`read()`方法时，我们称它为阻塞（Blocking）的，意思是，对于这一个方法，我们不知道要花费多少时间，代码必须要等该方法结束后才能继续运行

### InputStream实现类
用`FileInputStream`可以从文件中获取输入流，此外，还可以使用`ByteArrayInputStream`在内存中模拟一个`InputStream`
`ByteArrayInputStream`实际上是把一个`byte[]`数组在内存中变成一个`InputStream`，实际应用不多，但是测试的时候可以用它构造一个`Inputstream`
我们传参的时候就只接受`InputStream`抽象类型，而不是具体的`FileInputStream`类型，使得代码可以处理`InputStream`的任意实现类

## OutputStream
与`InputStream`类似，但是是输出流，也是一个抽象类，其中一个最重要的方法就是`write(int b)`
```java
public abstract void write(int b)throws IOException;
```
这个只会写入一个字节，也就是只写入`int`中表示低八位的部分（`b&0xff`）
与`InputStream`类似，`OutputStream`也提供了`close()`方法
需要特别注意的是：`OutputStream`提供了一个`flush()`方法，将缓冲区的内容强制输出到目的地（与c++类似）

与`InputStream`类似，也有重载方法可以一次写入多个字节：
```java
public void writeFile()throws IOException{
	try(OutputStream output=new FileOutputStream("C:\\code\\java\\exp.txt")){
		output.write("Hello".getBytes("UTF-8"));
	}
}
```
### 阻塞
与`InputStream`一样
### OutputStream实现类
与`InputStream`类似
`try(resource)`中的resource可以一次性操作多个资源

## Filter模式
可以发现如果给`FileInputStream`添加3种功能，至少需要3个子类，而这几种功能结合起来又有更多的子类
因此很快就会发生子类爆炸的现象
直接使用继承，为`InputStream`附加更多的功能，根本无法控制代码的长度和复杂度，很快就会失控
为了解决这种情况，JDK将`InputStream`分成了两大类：
一类是直接提供数据的基础`InputStream`：
- `FileInputStream`
- `ByteArrayInputStream`
- `ServletInputStream`
- ...

一类是提供额外功能的`InputStream`:
- `BufferedInputStream`
- `DigestInputStream`
- `CipherInputStream`
- ...

之后如果我们需要给一个基础`InputStream`附加各种功能时，我们首先先确定数据来源，例如文件：
```java
InputStream fileinput=new FileInputStream("test.gz");
```
然后我们还想提供缓冲的功能来提高读取速度，因此我们用`BufferedInputStream`来包装`InputStream`
```java
InputStream bufferedinput=new BufferedInputStream(fileinput);
```
最后假设该文件已被gzip压缩过了，我们希望能够直接读取其内容：
```java
InputStream gzipiput=new GZIPInputStream(bufferedinput);
```
而这仍被视为一个`InputStream`

通过上述这种一个“基础”组件再叠加各种”附加“功能的模式，称为Filter模式（或者装装饰器模式：Decorator)

### 编写FilterInputStream
我们也可以自己编写一个自己的`FilterInputStream`，以便将其“叠加”到任何一个`InputStream`
下面是一个对输入的字节进行计数的例子：
```java
class CountInputStream extends FilterInputStream{
	private int count=0;
	CountInputStream(InputStream in){
		super(in);
	}
	
	public int getByteRead(){
		return this.count;
	}

	public int read()throws IOException{
		int n=in.read();
		if(n!=-1)this.count++;
		return n;
	}

	public int read(byte[] b,int off,int len)throws IOException{
		int n=in.read(n.off,len);
		if(n!=-1)this.count+=n;
		return n;
	}
}
```

## 操作Zip

## 读取classpath资源

## 序列化
序列化就是将一个java对象变成二进制内容，本质上就是一个`byte[]`数组。
将对象序列化之后就可以把`byte[]`保存到文件中，或者把`byte[]`通过网络传输到远程
有序列化就有反序列化，也就是把一个二进制内容变回java对象

如何将一个对象序列化？
一个Java对象想要序列化，必须实现一个特殊的`java.io.Serializable`接口，定义如下：
```java
public interface Serializable{
}
```
`Serializable`接口没有定义任何方法，它是一个空接口。我们将这样的接口称为“标记接口”（Marker Interface）,实现标记接口的类仅仅是给自己贴了个“标记”，并没有增加任何的方法

### 序列化
把一个Java对象变为`byte[]`数组，需要使用`ObjectOutputStream`，它负责把一个Java对象写入一个字节流：
```java
import java.io.*;
import java.util.Arrays;

public class Main{
	public static void main(String argc[])throws IOException{
		ByteArrayOutputStream buffer=new ByteArrayOutputStream();
		try(ObjectOutputStream output=new ObjectOutputStream(buffer)){
			output.writeInt(12345);
			output.writeUTF("Hello");
			output.writeObject(Double.valueOf(123.45));
		}
		System.out.println(Arrays.toString(buffer.toByteArray()));
	}
}
```
`ObjcetOutputStream`既可以写入基本类型，也可以写入`String`（以`UTF-8`编码），也可以写入实现了`Serializable`接口的`Object`
因为写入`Object`时需要大量的类型信息，所以写入的信息量比较大

### 反序列化
和`ObjectOutputStream`相反，`ObjectInputStream`负责从一个字节流读取Java对象
除了能读取基本类型和`String`外，调用`readObject()`可以返回一个`Object`对象，要把它变成一个特定类型，必须强制转型
`readObject()`可能抛出的异常包括：
- `ClassNotFoundException`：没有找到对应的`Class`
- `InvalidClassException`：Class不匹配

对于`ClassNotFoundExceoption`没什么好说的
对于`InvalidClassException`常见于序列化的`Person`对象定义了一个`int`类型的`age`字段。但是反序列化时，`Person`类定义的`age`字段被改成了`long`，所以导致不兼容
为了避免这种class发生变动导致的不兼容，java的序列化允许class定义一个特殊的`serialVersionUID`静态变量，用于标识Java类的序列化“版本”，通常由IDE自动生成。如果增加或修改了字段，可以改变`serialVersionUID`的值，这样就能自动阻止不匹配的class版本
```java
public class Person implements Serializable{
	private static final long serialVersionUID=1234587231456L;
}
```
反序列化时，由JVM直接构造出Java对象，不调用构造方法，构造方法内部的代码不会被调用

### 安全性
因为Java的序列化机制可以导致一个实例能直接从`byte[]`数组创建，而不经过构造方法，因此，它存在一定的安全隐患。一个精心构造的`byte[]`数组被反序列化后可以执行特定的Java代码，从而导致严重的安全漏洞
实际上，Java本身提供的基于对象的序列化和反序列化机制既存在安全性问题，也存在兼容性问题。更好的序列化方法是通过JSON这样的通用数据结构来实现，只输出基本类型（包括String）的内容，而不存储任何与代码相关的信息

