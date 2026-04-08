# java21-22-25-switch模式匹配
现在可以使用如下的语法：
```java
public static String test(String str){
	return switch (str) {	// 直接返回switch表达式的结果
		case "a" -> "A";
		case "b" -> "B";
		default -> "C";
	};
}
```

判断目标是否是某类型
```java
public static String test(Object obj){
	return switch (obj) {
		case String s -> "这是一个字符串，内容是：" + s;
		case Integer i -> "这是一个整数，值是：" + i;
		default -> "这是一个未知类型的对象";
	};
}
// 或者
public static String test(Object obj){
	String result = switch (obj) {
		case String s -> "这是一个字符串，内容是：" + s;	// 这里的s是一个新的变量，作用域仅限于这个case分支
		case Integer i -> "这是一个整数，值是：" + i;
		default -> "这是一个未知类型的对象";
	};
}


```
可以使用when等价的条件来进一步细化匹配，这种条件被称为“守卫条件”
```java
public static String test(Object obj){
	return switch (obj) {
		case String s when s.length() > 5 -> "这是一个长度大于5的字符串，内容是：" + s;
		case String s -> "这是一个长度不大于5的字符串，内容是：" + s;
		case Integer i when i > 100 -> "这是一个大于100的整数，值是：" + i;
		case Integer i -> "这是一个不大于100的整数，值是：" + i;
		default -> "这是一个未知类型的对象";
	};
}
```
这需要在java 24作为预览使用，在java 25中正式发布。

也可以使用记录类来进行模式匹配
```java
public record Point(int x, int y) {}

public static String test(Object obj){
	return switch (obj) {
		case Point(int x,int y) -> "这是一个点，坐标是：" + x + "," + y;	// 直接结构对象的属性来进行匹配
		default -> "这是一个未知类型的对象";
	};
}
```

还支持对记录类进行嵌套匹配。
