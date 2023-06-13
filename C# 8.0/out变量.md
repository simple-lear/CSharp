# out变量

out变量是一种用于在方法中返回多个值的机制，可以让方法返回多个值而不需要使用元组等手段。

可以声明一个或多个out变量，必须得在方法内部被赋值，否则会编译错误。



C#7之前使用out参数必须保证变量在用作实参之前已提前声明。

**out 参数的内联变量声明**

C#7允许在方法调用时声明变量

```C#
//C#6
public static int? ToStringAndLength(string text)
{
	int len;
	return int.TryParse(text, out len) ? len : null;
}
```

```C#
//C#7
public static int? ToStringAndLength(string text) =>
    int.TryParse(text, out int len) ? len : null;	//可以在方法调用时声明
```



out变量实参和模式匹配中引入的新变量在以下情况行为相似：

* 如果不关心变量值，可以在变量名前加下划线表示这是一个抛弃变量
* 可以使用var声明一个隐式类型的变量（通过形参的类型推断其类型）
* 表达式树中不能使用out变量实参
* 变量作用域局限于周围的代码块
* 在字段、属性、构造方法或者C#7.3之前的查询表达式中不能使用out变量
* 只有方法确定会被调用，out变量才会确定赋值。如果因为某运算符短路而未调用，则out不会被赋值





**C#7.3out变量和模式变量解除的限制**

初始化字段或者属性时不能使用模式变量，构造初始化器（this(...) 和 base(...)）或查询表达式时也不能使用。out变量同样有这些限制。但C#7.3解除了  

```C#
public class Person
{
	public string Name { get; set; }

	public bool IsValue { get; set; }

	public Person(string name,bool isValue)
	{
		Name = name;
		IsValue = isValue;
	}

}

public class Student : Person
{
	public int? Value { get; set; }
	public Student(string name,int value) : base(name,int.TryParse(name,out int result))
	{
		Value = IsValue ? result : null;
	}
}
```

