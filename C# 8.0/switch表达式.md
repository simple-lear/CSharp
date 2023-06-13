# switch 表达式

C#7：switch语句和模式匹配

C#8：switch表达式作为switch语句的一个可选项

```C#
static double Perimeter(Shape shape)
{
	return shape switch
	{
		null => throw new ArgumentNullException(nameof(shape)),
		Rectangle rect => 2 * (rect.Height + rect.Width),
         //表示匹配不成功
		_ => throw new ArgumentException($"Shape type {shape.GetType()} perimeter unknown", nameof(shape)),	
	};
}

static double Perimeter(Shape shape) => 
    shape switch
	{
		null => throw new ArgumentNullException(nameof(shape)),
		Rectangle rect => 2 * (rect.Height + rect.Width),
         //表示匹配不成功
		_ => throw new ArgumentException($"Shape type {shape.GetType()} perimeter unknown", nameof(shape)),	
	};
```



switch语句和表达式之间一个重要的区别：

* 表达式必须返回一个结果（或异常）

表达式的一个问题：无法表达多个模式指向同一个结果，语句中可以将多个case标签指向同一个代码块。



**嵌套模式匹配**

C#7中的模式匹配：

* 类型模式
* 常量模式（expression is 10 、expression is null）等
* var 模式
* 嵌套模式（C#8）：模式内部可以嵌套子模式，与分解模式类似





**index 和 range**

两个结构体

* Index：整型数，表示某个可索引值的起始或结尾。不能为负值
* Range：一对Index的组合，两个index分别表示range的起始值和终止值，可以是任意索引值。^5..10：从倒数第5个元素到正数第10个元素之间的范围

由此引出了3点重要的语法：

* 从int创建一个起始位置的 Index的普通隐式转换
* 新的一元运算符 ^ ，和一个int值连用，用于创建一个末尾位置的Index。0值表示刚刚跨过末尾元素。1表示最后一个元素
* 新的二元操作符..其操作数（可选），用于创建Range的起始值和终止值。准二元，操作数可以是0,1,2



**应用**

index 和 range 都可以应用于表示序列的类型

* 数组
* span
* 字符串（作为utf-16编码单元的序列）

它们都支持两种操作：

* 提取某个元素
* 创建原序列的子序列

```C#
public static void TestIndexAndRange()
{
	string text = "Hello World";
	Console.WriteLine(text[1]);     //下标从0开始
	Console.WriteLine(text[^3]);    //获取倒数第3个字符
	Console.WriteLine(text[2..7]);  //使用range获取子串
	Console.WriteLine(text[^5..]);  //获取最后5个字符

	Span<int> span = stackalloc int[] { 5, 2, 7, 8, 2, 4, 3 };
	Console.WriteLine(span[2]);
	Console.WriteLine(span[^3]);
}
```

