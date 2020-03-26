# Java学习笔记

---

##获取用户从控制台输入的数据
```java
package com.HelloWorld;
import java.util.Scanner;
/*
 *System.out.print与System.out.println的区别是：
 *print在用户输入数据后不会换行，而println会换行。
 */
public class HelloWorld {
	public static void main(String[] args){
		Scanner input=new Scanner(System.in);
		System.out.print("请输入考试成绩信息:");
		
		int score=input.nextInt();
		int count=0;
		
		System.out.println("加分前的成绩为："+score);
		for(;score<60;score++,count++){	}
		System.out.println("加分后的成绩为："+score);
		System.out.println("加分的次数为："+count);
	}
}
```
##数组的声明，空间的分配及赋值
```java
/*声明*/
//以下两种方法等价
int[] scores;
int scores[];

/*分配*/
scores=new int[5];
int[] scores=new int[5];

/*赋值*/
//注意new后面的int[]里的[]不能指定长度！！！！
int[] scores=new int[]{78,91,84,68};
```
##使用Arrays类操作Java中的数组
```java
import java.util.Arrays

public class HelloWorld{
	public static void main(String[] args){
		int[] scores=new int[]{5,4,3,2,1};
		//使用排序方法
		Arrays.sort(scores);
		//输出排序结果
		System.out.println("排序完后的结果：");
		for(int i=0;i<scores.length;i++)
		{
			System.out.print(scores[i]);
		}
		//使用转换为字符串方法并输出
		System.out.print("将数组变成字符串："+Arrays.toString(scores));
	}
}

```

##Java中二维数组的定义
```java
/*要注意到声明时候没有指定行列的数值*/
//数据类型 [][] 数组名称 = new 数据类型 [长度][长度] ;  
//数据类型 [][] 数组名称 = {{123},{456}} ;  
```
##Java中局部变量与成员变量
> Java中成员变量可以不用赋初值，编译器默认为0；而局部变量一定要赋初值，否则编译器会报错。

> 这一点与C++有区别！！！

##Java中的静态初始化块
```java
public class Telephone {
	float screen;
	static float cpu;
	static float memeroy;
	//初始化块
	{
		screen=5;
	}
	//静态初始化块
	static{
		cpu=1.5f;
		memeroy=2.0f;
	}
}

```
##Java中的成员内部类
```java
public class outer{
	int oValue=1;
	//内部类
	public class inner{
		int iValue=2;
		public void print()
		{
			System.out.println("访问外部成员变量oValue:"+outer.this.oValue);//访问外部类成员变量
			System.out.println("访问内部成员变量iValue:"+iValue);
		}
	}
    	//外部方法
    	public void GetInnerPrint()
    	{
    	    new inner().print();
    	}
    	
    	//主函数
    	public static void main(String[] args)
    	{
    	    //创建外部类的对象
		    outer o=new outer();
            //创建内部类的对象
		    Inner i=o.new Inner();
		    //调用内部类对象的print方法
		    o.GetInnerPrint();
            //调用内部类对象的print方法
		    i.print();
    	}
}
//值得注意的是，内部类可以直接访问外部类的成员变量或者方法，而相反地，外部类不能直接访问内部类的成员变量或者方法，而要通过new一个内部类的对象来访问
```

##Java中的关键字
```
final
super
instanceof
```