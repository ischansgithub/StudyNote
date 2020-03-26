# 《深入理解C指针》学习笔记
---


##2016/12/21
-  sprintf与snprintf(在5.3.3 传递需要初始化的字符串 中遇到)
```
#include <stdio.h>
/*sprintf()函数用于将格式化的数据写入字符串，其原型为：*/
int sprintf(char *str, char * format [,argument, ...]);

/*example*/
char buf[10];
sprintf(buf, "The length of the string is more than 10");
printf("%s", buf);

/*snprintf()函数用于将格式化的数据写入字符串，其原型为：*/
/*与sprintf的区别是snprintf可以控制要打印的字符串的长度N*/
int snprintf(char *str, int n, char * format [, argument, ...]);
/*example*/
char str[5];
int ret = _snprintf(str,3,"%s","abcdefg");
printf("%d\n",ret);
printf("%s",str);
return 0;

```
##2016/12/20

-  有关字符指针的初始化C
```
char* character='c';//不合法
//可以这么做
char* character="c";//但有警告。 deprecated conversion from string constant to 'char*'
//或者这么做
char* character=(char*)malloc(50);
*character='c';
*(character+1)='w';

```
##2016/12/15
-  函数指针
```
#include<iostream>

/*定义一个指向函数的指针*/
typedef int (*fptrOperation)(int,int);
/*加法*/
int add(int num1,int num2)
{
	return num1+num2;
}
/*减法*/
int subtract(int num1,int num2)
{
	return  num1-num2;
}
/*函数指针作参数*/
int compute(fptrOperation po,int num1,int num2)
{
	return po(num1,num2);
}
/*函数返回一个函数指针*/
fptrOperation selectInput(char code)
{
	switch(code)
	{
		case '+':return add;
		case '-':return subtract;
	}
}
/*传入一个字符，函数局部定义一个函数指针*/
int evaluate(char code,int num1,int num2)
{
	fptrOperation fpc=selectInput(code);
	return fpc(num1,num2);
}

int main()
{
	char key;
	printf("please input '+'or '-':");
	scanf("%c",&key);
	printf("%d\n",evaluate(key,6,4));
	
	fptrOperation fptAdd=&add;
	printf("%d",fptAdd(100,3));
	return 0;
}

```
##2016/12/9
-  传递指针的指针与动态内存分配
```
//确实有分配内存，但函数返回后，指针就销毁了。
void GetMemory(char* pc)  
{  
    pc = (char *)malloc(100);  
}
//方法一：返回地址。
char* GetMemory()  
{  
    char* pc = (char*)malloc(100);  
	return pc;
} 
//方法二：指向指针的指针
void GetMemory(char** pc)  
{  
    *pc = (char*)malloc(100);  
}

int main()
{
    //方法一：
	char *str = GetMemory(); //但是这样要记得free(str),否则会造成内存泄漏 
    GetMemory(str);   
    strcpy(str, "hello world");  
    printf(str);
    free(str);
    //方法二：
	char* str=NULL;
	GetMemory(&str);
	strcpy(str,"hello world");
	printf(str);  
	 
	return 0;
}
```
##2016/12/4
-  迷途指针：free后没有置空，则此指针称迷途指针
```
//没有置空的指针，可以再次访问及修改，但会造成不可预期的错误。但在VS2015编译运行并不会出错(可能小程序比较简单)
char* name=(char*)malloc(16);
strcpy(name,"chanweiwei");
free(name);
//name= NULL;//如果这句语句没有被注释，则编译会出错。
strcpy(name, "ischan");
printf("%s", name);
```

##2016/12/3

-  指针的算术运算:指针加减整数；指针相互加减；指针的比较。

-  const与指针：指向常量的指针；指向非常量的常量指针；指向常量的常量指针(很少用)

-  内存泄漏：
```
//1.丢失地址
char* myName=(char*)malloc(strlen("chan")+1);
strcpy(myName,"chan");
while(*myName)
{
    printf("%c",*myName);
    myName++;
}
//2.没有调用free
```

-  初始化静态或全局变量时不能调用函数
```
/*书上这么讲*/
static int* pi=malloc(sizeof(int));//错误写法

static int* pi;//正确写法
pi=malloc(sizeof(int));
```

```
//实际上
//确实是错误写法，invalid conversion from 'void*' to 'int*' 
static int* pi=malloc(sizeof(int));
//这么写可以编译成功
static int* pi=(int*)malloc(sizeof(int));
```

-  calloc:在动态分配完内存后，自动初始化该内存空间为零(即指向的内容清零)
```
//原型
void* calloc(size_t numElements,size_t elementSize)

int* pi=(int*)calloc(5*sizeof(int));
//等价于
int* pi=(int*)malloc(5*sizeof(int));
memset(pi,0,5*sizeof(int));
```

-  发现书上错误
P33页，倒数第二行*pc[i]=0,应该改为pc[i]=0

##2016/11/30
-  任何指针都可以被赋给void指针。
```
int* pi=&num;
void* pv=pi;
pi=(int*)pv;
```
-  intptr_t与uintptr_t

-  size_t与%zu
```
size_t sizet=-5;
printf("%d\n",sizet); //输出-5
printf("%zu\n",sizet);//输出429496729

size_t sizet=5;
printf("%d\n",sizet); //输出5
printf("%zu\n",sizet);//输出5
```




