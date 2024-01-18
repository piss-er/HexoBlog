---
title: C Sharp初级语法
categories:
- 技术笔记
tags:
- C#
- 游戏开发
---

### C#简介

> C# 是一个现代的、通用的、面向对象的编程语言，它是由微软（Microsoft）开发的，由 Ecma 和 ISO 核准认可的。
> C# 的构想十分接近于传统高级语言 C 和 C++，是一门面向对象的编程语言，但是它与 Java 非常相似。

    下面列出 C# 一些重要的功能：

        - 布尔条件（Boolean Conditions）
        - 自动垃圾回收（Automatic Garbage Collection）
        - 标准库（Standard Library）
        - 组件版本（Assembly Versioning）
        - 属性（Properties）和事件（Events）
        - 委托（Delegates）和事件管理（Events Management）
        - 易于使用的泛型（Generics）
        - 索引器（Indexers）
        - 条件编译（Conditional Compilation）
        - 简单的多线程（Multithreading）
        - LINQ 和 Lambda 表达式
        - 集成 Windows


---

### C#的基本程序结构

#### 先来一个Hello World

```C#
using System;   // 在程序中包含 System 命名空间, 即可以直接使用System中的方法
namespace HelloWorldApplication     // 一个 namespace 里包含了一系列的类
{
    // HelloWorldApplication 命名空间包含了类 HelloWorld
    class HelloWorld
    {
        static void Main(string[] args)     // Main方法，是所有C#程序的入口点
        {
         /* 我的第一个 C# 程序*/
         Console.WriteLine("Hello World");  // WriteLine 是一个定义在 System 命名空间中的 Console 类的一个方法
         Console.ReadKey();     // 最后一行 Console.ReadKey(); 是针对 VS.NET 用户的。这使得程序会等待一个按键的动作，防止程序从 Visual Studio .NET 启动时屏幕会快速运行并关闭。
        }
   }
}
```
> - C# 是大小写敏感的。
> - 所有的语句和表达式必须以分号（;）结尾。
> - 程序的执行从 Main 方法开始。
> - 与 Java 不同的是，文件名可以不同于类的名称。
> - C# 代码文件的后缀名为cs, (C sharp)

#### 再来看一个矩形类 Rectangle

```C#
using System;
namespace RectangleApplication
{
    class Rectangle
    {
        // 成员变量
        double length;
        double width;
        public void Acceptdetails()
        {
            length = 4.5;    
            width = 3.5;
        }
        public double GetArea()
        {
            return length * width;
        }
        public void Display()
        {
            Console.WriteLine("Length: {0}", length);
            Console.WriteLine("Width: {0}", width);
            Console.WriteLine("Area: {0}", GetArea());
        }
    }
    
    class ExecuteRectangle
    {
        static void Main(string[] args)
        {
            Rectangle r = new Rectangle();
            r.Acceptdetails();
            r.Display();
            Console.ReadLine();
        }
    }
}
```

> 尝试运行上面代码, 结果如下

    Length: 4.5
    Width: 3.5
    Area: 15.75

### C#基础语法

#### using关建字

> 在任何C#程序中的第一条语句都是`using System;`
> `using`关建字用于包含命名空间, 即可以使用指定命名空间中的方法
> 一个程序可以包含多个using语句

#### class关建字

> 用于声明一个类, 与Java不同的是, 文件名可以不同与类的名称

#### 注释

```c#
// 单行注释
/*
多行注释
*/
```

#### 成员变量

> 类中的变量, 可以是基本类型的变量, 也可以是自定义类型的对象
> 在[Rectangle](#再来看一个矩形类-rectangle)中, `length`和`width`就是两个成员变量

#### 成员函数

> 类中执行指定任务的语句
> 在[Rectangle](#再来看一个矩形类-rectangle)中, `AcceptDetails`和`Display`等就是成员函数

#### 标识符

> 用来命名类、变量、函数等
- 可以包含数字, 字母, 下划线`_`和`@`
- 数字不能开头
- 不能是C#关键字

#### 数据类型

在C#中变量有3种类型
- 值类型
- 引用类型
- 指针类型

> 基本和C++相同

##### 值类型

> 例如`int`,`char`,`float`等, 可以直接分配一个值
> 基本和C++相同, 不同类型占用不同内存空间

> 例如下面例子

```c#
using System;

namespace DataTypeApplication
{
   class Program
   {
      static void Main(string[] args)
      {
         Console.WriteLine("Size of int: {0}", sizeof(int));
         Console.ReadLine();
      }
   }
}
```

> 运行结果如下

    Size of int: 4

> 同时从上述例子中可以看出, 在C#的的打印方法`console.WriteLine()`中可以使用{0}格式化字符串

##### 引用类型

> 变量指向内存中的一个位置, 例如`string`

> 常见的引用类型:

- 对象类型
- 动态类型

> 动态类型示例(动态类型的类型检查是在运行时发生的)

```C#
dynamic d = 20;
```
> 字符串`string`示例

```C#
// 创建一个字符串
string str = "runoob.com";
// 转义字符\
string str = "C:\\Windows";
// 逐字字符串,@,使用@之后,后面的字符串每一个都是普通字符,平等看待
string str = @"C:\Windows"
// @逐字字符串后面的空格以及换行符也都是普通字符, 同一个字符串可以换行
string str = @"<script type=""text/javascript"">
    <!--
    -->
</script>";
```

##### 指针类型

> 基本和C++一样, 指向一个内存空间, 如果指向对象的内存空间的话, 访问其成员使用`->`

```C#
char* cptr;
int* iptr;
```

#### 类型转换

>**注意事项**
- 隐式转换只能将较小范围的数据类型转换为较大范围的数据类型
- 显式转换可能会导致数据丢失或精度降低，需要进行数据类型的兼容性检查
- 对于对象类型的转换，需要进行类型转换的兼容性检查和类型转换的安全性检查


##### 隐式类型转换

```C#
byte b = 10;
int i = b;      // 低精度向高精度转换, 不需要显式转换
int intValue = 42;
long longValue = intValue;  // 低精度向高精度
```

##### 显式转换

```C#
int i = 10;
byte b = (byte)i;   // 高精度向低精度, 需要使用强制类型转换
double doubleValue = 3.14;
int intValue = doubleValue;     // 高精度向低精度
```

> 强制转换为字符串

```C#
int intValue = 123;
string stringValue = intValue.To.String();
```

C#提供的类型转换方法
|方法名|作用|
|:---|:---|
|ToBoolean|如果可能的话, 把类型转换为布尔型|
|ToByte|把类型转换为字节类型|
|ToChar|把类型转换为单个Unicode字符类型|
|ToDateTime|把整数或者字符串转换为`日期-时间`结构|
|ToDecimal|把浮点型或整数类型转换为十进制类型|
|Todouble|把类型转换为双精度浮点型|
|ToInt16|把类型转换为16位整数类型|
|ToInt32|把类型转换为32位整数类型|
|ToInt64|把类型转换为64位整数类型|
|......|......|

```C#
namespace TypeConversionApplication
{
    class StringConversion
    {
        static void Main(string[] args)
        {
            int i = 75;
            float f = 53.005f;
            double d = 2345.7652;
            bool b = true;

            Console.WriteLine(i.ToString());
            Console.WriteLine(f.ToString());
            Console.WriteLine(d.ToString());
            Console.WriteLine(b.ToString());
            Console.ReadKey();
            
        }
    }
}
```

> 结果如下

    75
    53.005
    2345.7652
    True

#### 接收来自用户的值

`Console.ReadLine()`可以以字符串格式接收数据

```C#
// 接收其他类型示例
int num;
num = Convert.ToInt32(Console.ReadLine());
```

`Convert.ToInt32()`可以把用户输入的数据转换为 int 数据类型

#### 常量

```C#
// 常见的数值型常量的后缀情况
int intValue = 10;
long longValue = 100L;
uint uintValue = 100U;
ulong ulongValue = 100UL;
float floatValue = 100.5F;
double doubleValue = 100.5;         // 后缀为D(d)或者不写
decimal decimalValue = 100.5M;      // 十进制数, 后缀为M
decimal decimalValue3 = 1E-5;       // 指数写法
```

> double和decimal有什么区别?
> - double是双精度浮点数类型，使用**64**位来存储数据，可以表示15位数字的精度。在声明double类型的变量时，可以使用后缀D或d来指定值的类型。例如：double num = 100.5D;
> - decimal是十进制浮点数类型，使用**128**位来存储数据，可以表示更高的精度。在声明decimal类型的变量时，可以使用后缀M或m来指定值的类型。例如：decimal num = 100.5M;

```C#
double doubleValue = 0.1 + 0.2;
Console.WriteLine(doubleValue);  // 输出：0.30000000000000004

decimal decimalValue = 0.1M + 0.2M;
Console.WriteLine(decimalValue);  // 输出：0.3
```

> 定义常量`const double pi = 3.14159;`

#### 运算符

假设假设变量 A 的值为 10，变量 B 的值为 20, 常见的**算术运算符,关系运算符和逻辑运算符**如下

| 运算符 | 描述           | 实例                  |
| :----- | :------------ | :-------------------- |
| +      | 把两个操作数相加 | A + B 将得到 30       |
| -      | 从第一个操作数中减去第二个操作数 | A - B 将得到 -10 |
| *      | 把两个操作数相乘 | A * B 将得到 200      |
| /      | 分子除以分母   | B / A 将得到 2        |
| %      | 取模运算符，整除后的余数 | B % A 将得到 0    |
| ++     | 自增运算符，整数值增加 1 | A++ 将得到 11   |
| --     | 自减运算符，整数值减少 1 | A-- 将得到 9    |
|==	|检查两个操作数的值是否相等，如果相等则条件为真。	|(A == B) 不为真。
|!=	|检查两个操作数的值是否相等，如果不相等则条件为真。	|(A != B) 为真。
|>	|检查左操作数的值是否大于右操作数的值，如果是则条件为真。	|(A > B) 不为真。
|<	|检查左操作数的值是否小于右操作数的值，如果是则条件为真。	|(A < B) 为真。
|>=	|检查左操作数的值是否大于或等于右操作数的值，如果是则条件为真。|(A >= B) 不为真。
|<=	|检查左操作数的值是否小于或等于右操作数的值，如果是则条件为真。	|(A <= B) 为真。
|&&	|称为逻辑与运算符。如果两个操作数都非零，则条件为真。	|(A && B) 为假。
|\|\||称为逻辑或运算符。如果两个操作数中有任意一个非零，则条件为真。	|(A \|\| B) 为真。
|!	|称为逻辑非运算符。用来逆转操作数的逻辑状态。如果条件为真则逻辑非运算符将使其为假。	|!(A && B) 为真。

**位运算符示例**

假设如果 A = 60，且 B = 13，现在以二进制格式表示，它们如下所示：
A = 0011 1100
B = 0000 1101


|运算符|	描述	|实例|
|:---|:---|:---|
|&	|如果同时存在于两个操作数中，二进制 AND 运算符复制一位到结果中。	|(A & B) 将得到 12，即为 0000 1100
|\|	|如果存在于任一操作数中，二进制 OR 运算符复制一位到结果中。	|(A \| B) 将得到 61，即为 0011 1101
|^	|如果存在于其中一个操作数中但不同时存在于两个操作数中，二进制异或运算符复制一位到结果中。|	(A ^ B) 将得到 49，即为 0011 0001
|~	|二进制补码运算符是一元运算符，具有"翻转"位效果。	|(~A ) 将得到 -61，即为 1100 0011，2 的补码形式，带符号的二进制数。
|<<	|二进制左移运算符。左操作数的值向左移动右操作数指定的位数。	|A << 2 将得到 240，即为 1111 0000
|>>	|二进制右移运算符。左操作数的值向右移动右操作数指定的位数。	|A >> 2 将得到 15，即为 0000 1111

**杂项运算符**

C#中的其他运算符

|运算符|	描述|	实例|
|:---|:---|:---|
|sizeof()	|返回数据类型的大小。	|sizeof(int)，将返回 4.
|typeof()	|返回 class 的类型。	|typeof(StreamReader);
|&	|返回变量的地址。	|&a; 将得到变量的实际地址。
|*	|变量的指针。	|*a; 将指向一个变量。
|? :	|条件表达式	|(1>0)?12:13
|is	|判断对象是否为某一类型。	|If( Ford is Car) // 检查 Ford 是否是 Car 类的一个对象。
|as	|强制转换，即使转换失败也不会抛出异常。	|Object obj = new StringReader("Hello");<br>StringReader r = obj as StringReader;

#### 判断

> `if-else`,`switch`和其它语言一样

#### 循环

> `while`,`do-while`,`for`,`break`,`continue`用法和其他语言一样
