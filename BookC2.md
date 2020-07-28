# Chapter 2. C# Language Basics

## Type Basics

> All C# types fall into the following categories:
>
> **1.Value types** 
>
> **2.Reference types** 
>
> **3.Generic type parameters** 
>
> **4.Pointer types**

### Value Types

- **Numeric**         

  ------

  ​		Predefined

  - Signed integer (`sbyte`, `short`, `int`, `long`)         
  - Unsigned integer (`byte`, `ushort`, `uint`, `ulong`)         

  - Real number (`float`, `double`, `decimal`)       

    -------

    Custom

  - `enum`

  - `struct`

  > **1.Numeric suffixes**
  >
  > <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728200507520.png" alt="image-20200728200507520" style="zoom:80%;" />
  >
  > **2. Range**
  >
  > <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728200951166.png" alt="image-20200728200951166" style="zoom: 67%;" />
  >
  > **3.e.g.** 
  >
  > 十进制:`int x = 127`
  >
  > 十六进制: `long y = ox7F`
  >
  > 二进制:`var b = 0b1010_1011_1100_1101_1110_1111;`

- Logical (`bool`)       

- Character (`char`)       

> 自定义一个值类型（struct)里面有两个int，每个Int 4 byte(32 bit) 就消耗4 byte +4 byte = 8 byte自定义一个引用类型多消耗4或者8 byte (depends on 32 or 64 bit OS）

**Reference types**       

- `string` 

- `object`
- `delegate`
- `interface` 
- `array`
- `dynamic`

### **Others**

- Conversion

  > type conversions are *implicit* when the destination type can represent every possible value of the source type. Otherwise, an *explicit* conversion is required. For example
  >
  > ```
  > int x = 12345; // int is a 32-bit integer
  > long y = x; // Implicit conversion to 64-bit integral type
  > short z = (short)x; // Explicit conversion to 16-bit integral type
  > ```
  >
  > A` float` can be implicitly converted to a `double`, since a `double` can represent every possiblevalue of a` float`. The reverse conversion must be explicit.
  >
  > 所有整型都可以隐式转换为decimal，其他类型需要显式转换
  >
  > All integral types may be implicitly converted to all floating-point
  >
  > ```csharp
  > int i = 1;
  > float f = i;
  > //The reverse conversion must be explicit:
  > int i2 = (int)f;
  > ```
  >
  > 

- 可以在数字中任意位置加下划线

- 加减乘除运算符，8bit/16bit的Numeric Type不能用,(加减等)运算时自动隐式转换成int

- Overflow

  - 溢出不会报异常

  - e.g.

    ```
    int a = int.MinValue;
    a--;
    Console.WriteLine (a == int.MaxValue); // True
    ```

  - 检查溢出:`checked`

    ```csharp
    nt a = 1000000;
    int b = 1000000;
    int c = checked (a * b); // Checks just the expression.
    checked // Checks all expressions
    { // in statement block.
    ...
    c = a * b;
    ...
    }
    ```

  - <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728203536370.png" alt="image-20200728203536370" style="zoom:67%;" />

## **Variables and Parameters**

A variable represents a storage location that has a modifiable value. A variable can be a ***local variable***, ***parameter*** (*value*, *ref*, or *out*), ***field*** (*instance* or *static*), or ***array element***.

local variables需要先赋值再用，field不需要，数组自动赋值为0

#### **Parameters**

传入值类型不改变原来的对象，传入引用类型改变引用的对象的值，但是将引用对象的变量设为null，其他引用此对象的变量不会也变成null

```csharp
        static void Foo(Point point)
        {
            point.X = 10;       //will change the referenced object's value
            point = null;       //we lose a copy of reference 'point',but the reference 'p1' outside still exist
        }
        static void Main()
        {
            Point p1 = new Point();
            Foo(p1);
            Console.WriteLine(p1.X);
        }
    }
    class Point
    {
        public int X { get; set; } = 5;
    }
```

使用`ref`关键字可以将外面的引用对象置为null

```csharp
        static void Foo(ref Point point)
        {
            point.X = 10;      
            point = null;      
        }
        static void Main()
        {
            Point p1 = new Point();
            Foo(ref p1);
            Console.WriteLine(p1.X);    //System.NullReferenceException: 'Object reference not set to an instance of an object.'
        }
    }
    class Point
    {
        public int X { get; set; } = 5;
    }
```

##### **The `ref` modifier**

传入一个引用，定义的变量需要本来有值

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728203922372.png" alt="image-20200728203922372" style="zoom:67%;" />

##### **The `out` modifier**

传入一个变量的引用，在函数里改变这个变量，用来变相return多个值。本来这个变量可以指向null，但是传入某个函数之后必须要给他赋值。

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728203945083.png" alt="image-20200728203945083" style="zoom:67%;" />

可以在调用函数的时候声明要out的变量的类型（就不需要提前定义好指向null的变量），可以用`out __`来diacard out变量。

![image-20200728204140545](D:\Desktop\Grocery\TyporaImageBackUp\image-20200728204140545.png)

##### **The `params` modifier**

作为方法的最后一个参数，这样方法就可以接受任意个数的参数，可以将`params`看作一个普通的数组。

![image-20200728204258196](D:\Desktop\Grocery\TyporaImageBackUp\image-20200728204258196.png)

##### **Optional parameters**

**定义函数的时候可以给参数赋默认值，变为optional参数，默认值必须是一个constant expression ，不能用ref或out修饰**

> Mandatory parameters must occur *before* optional parameters in both the method declarationand the method call (the exception is with params arguments, which still always come last).

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728204436873.png" alt="image-20200728204436873" style="zoom:67%;" />

##### **Named arguments**

用冒号指定赋给哪个参数，named arguments必须放在positional arguments 之后，调用的时候参数的顺序可以任意

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728204513361.png" alt="image-20200728204513361" style="zoom:67%;" />

## **Null Operators**

##### **Null Coalescing Operator:双问号**

双引号左边的如果是null,使用右边的值

```csharp
string s1 = null; 
string s2 = s1 ?? "nothing";   // s2 evaluates to "nothing"
```

##### **Null-conditional Operator (C# 6)：单问号加点**

如果单引号加点左边是null，表达式为Null而不是throw 一个NullReferenceException

```csharp
System.Text.StringBuilder sb = null; 
string s = sb?.ToString();      // No error; s instead evaluates to null
```

```cs
someObject?.SomeVoidMethod();     //If someObject is null, this becomes a “no-operation” rather than throwing a NullReferenceException.
```

```csharp
System.Text.StringBuilder sb = null;
string s = sb?.ToString() ?? "nothing";   // s evaluates to "nothing"
```

## **Statements**

##### **Local variables**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728204939723.png" alt="image-20200728204939723" style="zoom:50%;" />

##### **Selection Statements**

1.Selection statements (if, switch) 

2.Conditional operator (?:)

3.Loop statements (while, do..while, for, foreach)

##### **The `switch` statement**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728205025785.png" alt="image-20200728205025785" style="zoom: 80%;" />

**The switch statement with patterns (C# 7)**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728205046074.png" alt="image-20200728205046074" style="zoom:80%;" />

You can predicate a case with the` when` keyword:

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728205100806.png" alt="image-20200728205100806" style="zoom: 67%;" />

## **Namespaces**

两个等价的namespace语法

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728205144429.png" alt="image-20200728205144429" style="zoom:80%;" />

##### **The `using` Directive**

```csharp
using Outer.Middle.Inner; 
class Test {  
static void Main(){    
Class1 c;    // Don't need fully qualified name 
    } 
}
```

##### **`using static` (C# 6)**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728205237761.png" alt="image-20200728205237761" style="zoom:67%;" />

##### **Rules Within a Namespace**

1. Name scoping : 内层namespace的class可以用外层的namespace不需要qualification
2. Name hiding : If the same type name appears in both an inner and an outer namespace, the inner name wins. To refer to the type in the outer namespace, you must qualify its name.
3. Repeated namespaces : You can repeat a namespace declaration, as long as the type names within the namespaces don’t conflict，哪怕他们在两个不同的source file
4. Nested using directive : 可以在namespace里包含using，这样被using的namespace就在本namespace里可见。

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728205432817.png" alt="image-20200728205432817" style="zoom:67%;" />

##### **Aliasing Types and Namespaces**

可以引用一个namespace里的一部分，可以给他一个alias

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200728205455116.png" alt="image-20200728205455116" style="zoom:80%;" />

##### **Advanced Namespace Feature**

1.extern：来自两个assemblies的一个namespace含有两个同名的type

2.Namespace alias qualifiers：内层namespace的names会隐藏外层的names，有时候写全qualified name 也不行，用global::  P80