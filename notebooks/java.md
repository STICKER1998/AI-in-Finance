# Chapter 1 JAVA基本语言
## 1.1 第一个JAVA代码

```java
public class Welcome {

	public static void main (String[] args){
		System.out.println("hello world");
	}
}
```

`javac Welcome.java`

`java Welcome`

> [!WARNING]
> - 1.JAVA 对大小写敏感；
> - 2.关键字class的意思是类，JAVA是面向对象的语言，所有的代码必须处于类中间；
> - 3.源文件编译后，得到相应的字节码文件，编译器为每个类生成独立的字节码文件；
> - 4. main方法是JAVA应用程序的入口方法，格式固定`public static void main(String[] args){...}`;
>   5. 一个源文件可以包含多个类；
>   6. 每个语句必须以分号结束，回车不是语句结束的标志；
