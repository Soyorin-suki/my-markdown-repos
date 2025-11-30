# Lambda表达式
Lambda表达式是c++11中提出的一个语法糖，Lambda表达式是在调用或作为函数参数传递的位置处定义匿名函数的便捷方法。
主要介绍Lambda表达式的简单使用方法
## Lambda表达式定义
Lambda表达式通常也被称为Lambda函数、匿名函数
一般使用auto存储或不存储，可以使用functional<type>存储或使用decltype()获取类型
一般Lambda表达式的定义为`[capture list](parameter)mutable throw()-> return-type{statement}`
分别是：
- 捕获列表：在c++标准中也被称为Lambda导入器，捕获列表总是出现在Lambda表达式的开始处，[]是Lambda引出符，编译器通过Lambda引出符判断接下来的代码是否是Lambda表达式，捕获列表可以捕捉上下文中的变量
- 参数列表：与普通函数一致，特殊地，当参数列表为空时可以省略`()`
- 可变规则：mutable修饰符，默认情况下Lambda表达式是一个const函数，mutable可以取消其常量性。在使用该修饰符时，参数列表不可省略
- 异常说明：用于说明内部可能抛出异常
- 返回类型：与普通函数一致，在不需要返回值的时候可以连`->`一并省略，此外在返回类型明确的时候，也可省略此部分，使编译器自行推导
- Lambda函数体：与普通函数一致

### 捕获列表
Lambda表达式与普通函数的最大区别在于可以使用上下文中出现的变量，下面是常用的捕获列表介绍：
`[]`表示不捕获任何变量：
```c++
auto function=([]{
	cout<<"Hello world\n";
});
```
`[var]`表示按值传递方式捕获变量：
```c++
int num=5;
auto function=([num]{
	cout<<(num+5)<<endl;
});
```
`[=]`表示按值传递方式捕获所有父作用域的变量（包括this）：
```c++
int num=1,num2=2;
auto function([=]{
	cout<<num<<' '<<num2<<endl;
});
```
`[&]`表示按引用传递方式捕获所有父作用域的变量（包括this）
`[&var]`表示按引用传递方式捕获var变量
`[=,&var1,&var2]`表示拷贝与引用混合，按引用传递方式捕获var1和var2，其余父作用域变量按值传递

### 参数列表
与普通函数参数列表基本一致
### mutable
一般Lambda表达式是一个const函数，使用mutable可以使其可变
### 异常说明
异常说明，建议不使用
### 返回类型
Lambda表达式的返回类型会自行推导，如果没有return语句或只有一个return语句，则可以省略，

## Lambda表达式工作原理
编译器会把一个Lambda表达式生成一个一个类的匿名对象，并在类中重载运算符，实现`operator()`方法
e.g.
```c++
auto print=[]{cout<<"hello world"<<endl;};
```
编译器会将其翻译为
```c++
class print_class{
	public:
	void operator()(void)const{
		cout<<"hello world"<<endl;
	}
}
```
## 仿函数
当一个类具有函数的性质，重载了`operator()`运算符，那么这个类就是一个仿函数类
类似上述Lambda表达式工作原理中的类

## 后续拓展
### c++14
1. **泛型Lambda(Generic Lambda)** 使得Lambda表达式可以自行推导参数列表
	```c++
	auto add=[](auto a,auto b){
		return a+b;
	};
	```
2. **Lambda捕获初始化(Lambda Capture Initialization)** 使得可以在捕获列表中初始化变量
	```c++
	int x=10;
	auto lambda=[y=x+10](){cout<<y<<' '<<endl;};
	lambda();
	```
3. **`decltype`** 允许使用`decltype`对Lambda中进行返回类型推导
	```c++
	auto lambda=[](auto a,auto b)->decltype(a+b) {
		return a+b;
	};
	```
### c++17
`constexpr`关键字和`if constexpr`关键字对Lambda的支持

### c++20
Lambda引入了模版、`explicit`和对协程的支持




参考：https://www.cnblogs.com/plus301/p/17580628.html