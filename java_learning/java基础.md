# java学习
## java快速入门
### java历史
Java最早由詹姆斯高斯林(Java之父)(James Gosling)所创建。
Java介于编译型语言和解释型语言之间。

Java有三个版本：
- Java SE: Standard Edition
- Java EE: Enterprise Edition
- Java ME: Micro Edition

常见名词
- JDK: Java Development Kit
- JRE: Java Runtime Environment
- JVM: Java Virtual Machine

简单来说JRE是运行Java字节码的虚拟机，JDK是提供了编译器、调试器等开发工具的东西，可以用于将Java源码编译成字节码
JVM是一个虚构出来的计算机，一种规范，通过在实际的计算机上仿真模拟各类计算机功能实现
> https://javaguide.cn/java/jvm/jvm-intro.html

- JSR规范: Java Specification Request
- JCP组织: Java community Process

为了保证Java的规范性，SUN公司出台JSR规范用于标准化规范、JCP是审核规范的组织
一个JSR发布时会同时发布一个"参考实现"和"兼容性测试套件"
- RI: Reference Implementation
- TCK: Technology Compatibility Kit

### 搭建开发环境
Java程序必须运行在JVM上
JDK的bin目录下有很多可执行文件:
- java: 这个就是JVM，运行Java程序就是启动JVM，然后让JVM执行指定的编译后的代码；
- javac: Java的编译器，用于把Java源码文件`.java`编译为Java字节码文件`.class`
- jar: 用于把一组`.class`文件打包成一个`.jar`文件，便于发布
- javadoc: 用于从Java源码中自动提取注释并生成文档
- jdb: Java调试器

### 第一个Java程序
与c++不同之处：
`final`相当于c++里的`const`
`var`对应c++里的`auto`
`boolean`对应`bool`
基础类型以外全是引用类型
`String`对应`string`,且创建之后不可变
可以使用
```java
String a="""
a
b
"""//来创建多行字符串
```
### 流程控制
输出还行
`System.out.println()`表示输出并换行`System.out.print()`表示输出但是不换行`System.out,printf()`表示格式化输出，几乎与C一样

输入比较复杂
需要先`import java.util.scanner`之后再创建一个`Scanner`对象，之后再用`Scanner.nextline()`或`Scanner.nextInt()`等输入

引用类型判断相等需要用`equals()`，直接使用`==`会返回0，因为两个引用变量不是同一个会返回0，并且需要判断引用变量是否是`null`防止出现`NullPointerException`

`switch`表达式：
形如：
```java
switch(op){
	case 1->{
		System.out.print(1);
		//不需要break;
	}
	case 2->{
		System.out.print(22);
	}
}
```
新语法**没有穿透效应**

`while` `do while` `for`和c++里的一致
`for(int x:a)`允许这样写
### 数组操作

`Arrays`需要使用`import java.util.Arrays`
`Arrays.toString()`可以将数组转化为字符串
`Arrays.sort()`可以将数组升序排序

命令行参数与c++几乎一致
