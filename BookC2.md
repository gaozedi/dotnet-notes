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

--------

# Chapter 3 **Creating Types in C#**

## Class

最简形式

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803200852968.png" alt="image-20200803200852968" style="zoom:67%;" />

在class关键词之前，之后和大括号里的关键词:

![image-20200803200908488](D:\Desktop\Grocery\TyporaImageBackUp\image-20200803200908488.png)



## Fields

e,g,

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803200949463.png" alt="image-20200803200949463" style="zoom:67%;" />

- *readonly* modifier: 阻止一个field在创建后被修改，只能在定义field或者构造函数内assign field
- Field initialization is optional

## **Methods**

- A method’s signature must be unique within the type. A method’s signature comprises its name and parameter types in order (but not the parameter names, nor the return type).

- Expression-bodied methods:

  ```csharp
  int Foo (int x) { return x * 2; }
  int Foo (int x) => x * 2; 
  ```

  

### Local Method (C# 7.0)

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803201259190.png" alt="image-20200803201259190" style="zoom:67%;" />

> The local method (`Cube`, in this case) is visible only to the enclosing method (WriteCubes)，需要在外层函数调用内层函数

- Local methods can appear inside other function kinds, such as property accessors, constructors
- The `static` modifier is invalid for local methods. They are implicitly static if the enclosing method is static.

### Constructor

- 没有返回值，名字和enclosing type一样 

- 可以用 expression-bodied members：e.g.

  ```csharp
  public Panda (string n) => name = n;
  ```

- **Overloading constructors**:

  - 一个constructor可以call另一个，用`this`关键字，其中price还可以是一个表达式。

    ![image-20200803201556827](D:\Desktop\Grocery\TyporaImageBackUp\image-20200803201556827.png)

- C#自动生成一个无参数的constructor,但是一旦自己定义了constructor,就不自动生成

- Field initializations occur before the constructor is executed, and in the declaration order of the fields.

- constructor不一定是要public的，可以设为不设置为public后用一个类里的static method来生成object，e.g.

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803201727024.png" alt="image-20200803201727024" style="zoom:67%;" />



#### **Deconstructors (C# 7)**

原本给构造函数参数赋值创建对象，此时将赋的值返回

*A deconstruction method must be called Deconstruct, and have one or more out parameters, such as in the following class*

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803202023005.png" alt="image-20200803202023005" style="zoom:67%;" />

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803202045781.png" alt="image-20200803202045781" style="zoom:67%;" />

第二行相当于：

` rect.Deconstructor ( out var width, out var height );`

### **Object Initializers**

要么用initializers直接给field等赋值，要么用constructor<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803202243790.png" alt="image-20200803202243790" style="zoom:67%;" />

### **The `this` Reference**

1.用来引用这个对象本身：P88底

2.用来避免参数和自身的properties/field同名   

## **Properties**

`get`/`set`作为properties accessors，有一个隐含的参数value

### **Expression-bodied properties (C# 6, C# 7)**

e.g.1 一个readonly Property(相当于只有get)

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803202456693.png" alt="image-20200803202456693" style="zoom:67%;" />

e.g.2 一个既可以读又可以写的property

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803202510077.png" alt="image-20200803202510077" style="zoom:67%;" />

>  自动属性：相当于在field后面加{get;set}

### **Property initializers (C# 6)**

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803202603731.png" alt="image-20200803202603731" style="zoom:67%;" />

> 可以只含get，这样相当于创建了一个immutable (read-only) types

### **get and set accessibility**

get和set可以有不同的accessibility

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803202645515.png" alt="image-20200803202645515" style="zoom:67%;" />

## **Indexers**

像string有indexers可以访问里面的每个char，通常用来访问一个class/type里面的list/dictionary value.

To write an indexer, define a property called **this**, specifying the arguments in square brackets. For instance:参数列表用方括号而不像普通的函数一样用圆括号

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803210559205.png" alt="image-20200803210559205" style="zoom:67%;" />

如果不用set只要获取,可以用expression-body syntax

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803210628848.png" alt="image-20200803210628848" style="zoom:67%;" />

## **Constants:P93**

是一个static field，值永远不会变

A constant is declared with the `const` keyword and must be initialized with a value.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803210704601.png" alt="image-20200803210704601" style="zoom:50%;" />

Constants can also be declared local to a method.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200803210749402.png" alt="image-20200803210749402" style="zoom:50%;" />

## **Static Constructors**

1.A static constructor executes once per type, rather than once per instance. A type can define only one static constructor, and it must be parameterless and have the same name as the type

2.触发static constructor:实例化这个type/访问这个type里面的静态成员

3.如果有静态field initializer, Static field initializers run just before the static constructor is called

## **Static Classes**

静态类只能含有静态成员，例如 System.Console and System.Math class

## **Finalizers**

1.Finalizers are class-only methods that execute before the garbage collector reclaims the memory for an unreferenced object. The syntax for a finalizer is the name of the class prefixed with the ~ symbol:

2.From C# 7, single-statement finalizers can be written with expression-bodied syntax:

`~Class1() => Console.WriteLine ("Finalizing");`

## **Partial Types and Methods:P96**

1. allow type definition to be split — typically across multiple files.*A common scenario is for a partial class to be auto-generated from some other source (such as a Visual Studio template or designer), and for that class to be augmented with additional hand-authored methods.* For example:

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804213442338.png" alt="image-20200804213442338" style="zoom:67%;" />

2. 每个partial type's participant must have the partial declaration ，下面的是**错误**的：

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804213501559.png" alt="image-20200804213501559" style="zoom:67%;" />

3. Participants cannot have conflicting members. e.g. A constructor with the same parameters

4. each participant can independently specify interfaces to implement.5.The compiler makes no guarantees with regard to field initialization order between partial type declarations. 

### **Partial methods**

Partial methods must be **void** and are implicitly **private**.

**一般用在一部分是自动生成的，一部分是手写的情况**

**1.含两部分：definition, implemention**

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804213633804.png" alt="image-20200804213633804" style="zoom:67%;" />

## **Polymorphism**

1. This means a variable of type x can refer to an object that subclasses x.
2. 一个方法的参数是父类，可以传给他子类，参数是子类传给他父类不行(subclass is more compotent, can be used in scenarios require the less compotent one)

### **Casting and Reference Conversions**

an object reference can be 隐式转换成一个父类（**up cast**)，Explicitly **downcast** to a subclass reference

#### **Upcasting**

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804213833215.png" alt="image-20200804213833215" style="zoom:67%;" />

`Console.WriteLine (a == msft);    // True`

尽管引用a指向(引用)了一个子类对象，不能完全当成子类来用，因为a的reference type是父类,即使指向的object是子类，只能当作父类来用，但是可以之后成功的downcast:

e.g.

![image-20200804214018187](D:\Desktop\Grocery\TyporaImageBackUp\image-20200804214018187.png)

#### **Downcasting**

A downcast operation **creates a subclass reference** **from a base class reference**. 

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804214044352.png" alt="image-20200804214044352" style="zoom:67%;" />

子类的引用赋值给父类的引用，然后再downcast给另一个子类引用不可以。即Donwcast要求原先是那个子类，被upcast到父类之后可以downcast回来 

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804214110132.png" alt="image-20200804214110132" style="zoom:67%;" />

> 其中第四行输出为默认初始值0

If a downcast fails, an InvalidCastException is thrown

#### **The `as` operator**

The `as` operator performs a downcast that evaluates to **null** (rather than throwing an exception) if the downcast fails.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804214240884.png" alt="image-20200804214240884" style="zoom:67%;" />

#### **The `is` operator**

The `is` operator **tests whether a reference conversion would succeed**; in other words, whether an object derives from a specified class (or implements an interface). It is often used to test before downcasting.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804214348339.png" alt="image-20200804214348339" style="zoom:67%;" />

##### **The `is` operator and pattern variables (C# 7):P100**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804214426191.png" alt="image-20200804214426191" style="zoom:67%;" />

#### **Virtual Function Members**

- 定义格式如public virtual decimal xxx,定义了可以被子类用override 关键词改写，用来实现更加具体的功能。改写了之后不仅子类的这个成员值改变，其被upcast之后的指向父类的引用的这个值也改变

- **Methods**, **properties**, **indexers**, and **events** can all be declared, **Field** can not 

- The **signatures**, **return types**, and **accessibility** of the virtual and overridden methods must be identical. An overridden method can call its base class implementation via the `base `keyword

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804214603992.png" alt="image-20200804214603992" style="zoom:67%;" />

#### **Abstract Classes and Abstract Members**

1. 可以定义抽象类，只有抽象类里可以定义抽象成员(一般的类不能包含抽象方法等)，只有这些抽象成员都被override之后这个类才可以被实例化。（抽象类里只可以定义抽象函数，属性等可以是非抽象的，但是也可以被override). **Abstract members are like virtualmembers, except they don’t provide a default implementation.**

2. abstract methods have no actual code in them, and subclasses HAVE TO override the method. Virtual methods can have code, which is usually a default implementation of something, and any subclasses CAN override the method using the override modifier and provide a custom implementation.

#### **Hiding Inherited Members(`new` keyword)**

1. B是A的子类，A,B类里面有一样的成员C则：References to A (at compile time) bind to A.C References to B (at compile time) bind to B.C编译器会产生一个warning，可以用new关键词suppress warning

2. new vresus overried:P102底当一个override的对象up cast到父类的时候，这个对象实现的是override之后的功能，当一个new 对象upcast到父类的时候，这个对象实现的是之前父类里的功能。

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804215148482.png" alt="image-20200804215148482" style="zoom:67%;" />

#### **Sealing Functions and Classes**

1. 可以seal一个方法，seal一个类(seal类里的所有virtual function),In our earlier virtual function member example, we could have sealed `House`’s implementation of `Liability`, preventing a class that derives from `House` from overriding `Liability`, as follows:

   `public sealed override decimal Liability { get { return Mortgage; } }`

2. Although you can seal against overriding, you can’t seal a member against being hidden.

#### **The `base` Keyword**

1. Accessing an overridden function member from the subclass.

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804215611249.png" alt="image-20200804215611249" style="zoom:67%;" />

   > 现在访问的就不是被virtual过的，父类的`Liability`属性，同样也可以用来访问被hidden(用new)的成员，但是被new的成员也可以用直接cast到父类的方法来访问

2. 可以用来Calling a base-class constructor.

#### **Constructors and Inheritance**

**如果子类没写constructor,不会自动继承父类的constructor，父类的无参构造函数总是默认被调用**

1. 父类的构造方法不自动被继承，可以用base来访问

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804215928748.png" alt="image-20200804215928748" style="zoom:67%;" />

> Base-class constructors always execute first; this ensures that *base* initialization occurs before*specialized* initialization.

2. If a constructor in a subclass omits the base keyword, the base type’s parameterless constructor is implicitly called

   e.g.

   ![image-20200804220050297](D:\Desktop\Grocery\TyporaImageBackUp\image-20200804220050297.png)



##### **Constructor and field initialization order**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804220123927.png" alt="image-20200804220123927" style="zoom:67%;" />

子类class D构造函数接收到一个参数x时，调用父类的构造函数，把x+1作为参数。

#### **Overloading and Resolution：P106**

两个函数同名且一个的参数是另一个的子类，When an overload is called, the most specific type has precedence

e.g.

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200804222528465.png" alt="image-20200804222528465" style="zoom:67%;" />

### The object Type

1. object (System.Object) is the ultimate base class for all types. Any type can be upcast to object.

2. 可以用object写个stack类，里面有push,pop等方法，so we can Push and Pop instances of any type to and from the Stack

   e.g.

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200816220842030.png" alt="image-20200816220842030"  />

3. 尽管object是个引用类型，C# value types, such as **int**, can also be cast to and from object, and so be added to our stack. This feature of C# is called type unification and is demonstrated here:

   ```csharp
   stack.Push(3);
   int three = (int) stack.Pop();
   ```

   

#### **Boxing and Unboxing**

1. Boxing将一个值类型转换成引用类型（一个object class or an interface）：

   ```csharp
   int x = 9;
   object obj =x; //box the int.
   ```

   

2. Unboxing reverses the operation, by casting the object back to the original value type:

   ```csharp
   int y = (int)obj;     //unbox the int.
   ```

   

3. unboxing时， Runtime检查这个值是不是正确的类型（上面例子中括号内的int为被检查的值，合格obj本身就是指向int 类型，则正确）错误例子：

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200816221318416.png" alt="image-20200816221318416" style="zoom:80%;" />

   此时括号内的long被检查，不匹配真正的值，报 InvalidCastException

   正确例子：

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200816221341911.png" alt="image-20200816221341911" style="zoom:67%;" />

4. ![image-20200816221359368](D:\Desktop\Grocery\TyporaImageBackUp\image-20200816221359368.png)



##### **Copying semantics of boxing and unboxing**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200816221456798.png" alt="image-20200816221456798" style="zoom:80%;" />

#### **Static and Runtime Type Checkin**

1. 静态检查错误：Static type checking enables the compiler to verify the correctness of your program without running it. The following code will fail because the compiler enforces static typing:

   `int x = "5";`

2. Runtime检查错误：Runtime type checking is performed by the CLR when you downcast via a reference conversion or unboxing. For example:

   ```csharp
   object y = "5"; 
   int z = (int) y;          // Runtime error, downcast failed
   ```

   

#### **The GetType Method and typeof Operator**

1. All types in C# are represented at runtime with an instance of System.Type. There are two basic ways to get a `System.Type` object:
   - `Call `GetType` on the instance.
   - Use the `typeof` operator on a type name.

2. System.Type有许多属性，例如 type’s name, assembly, base type, and so on.

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200816221810597.png" alt="image-20200816221810597"  />

   

#### **The `ToString` Method**

1. The ToString method returns the default textual representation of a type instance

2. 可以在custome types里面override ToString方法，如果不override,则返回type name

#### **Object Member Listing**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200816221914081.png" alt="image-20200816221914081" style="zoom: 67%;" />

### **Structs**

1. struct和class类似，区别：
   - struct是值类型
   - struct不支持继承
   - A struct can have all the members a class can, **except** the following:
     -  A parameterless constructor
     - Field initializers 
     - A finalizer
     - Virtual or protected members

#### **Struct Construction Semantics**

1. struct有一个隐含的无参构造函数不能被override，所有的field自动被赋值为二进制0
2. 如果写了一个构造函数，必须把所有field赋值

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200816222117102.png" alt="image-20200816222117102" style="zoom:80%;" />

### **Access Modifiers**

- *`public`* Fully accessible. This is the implicit accessibility for members of an enum or interface. 

- *`internal`* Accessible only within the containing assembly or friend assemblies. This is the default accessibility for non-nested types. 

  > **N.B.**  Nested type, in C#, is a type declared within an existing class or struct. Unlike a non-nested type, which is declared directly within a compilation unit or a namespace 
  >
  > An assembly is (in general) a single .dll or .exe file. A C# project (in general) compiles to a single assembly.

- *`private`* Accessible only within the containing type. This is the default accessibility for members of a class or struct. 

- *`protected`* Accessible only within the containing type or subclasses. 

- *`protected internal`* The union of `protected` and `internal` accessibility. Eric Lippert explains it as follows: *Everything is as private as possible by default, and each modifier makes the thing more accessible.*

  So something that is protected internal is made more accessible in two ways.

- >    **Examples:P111**   
  >
  > **Friend Assemblies:P111 bottom,standby**

#### **Accessibility Capping**

类访问不到，类里的成员哪怕是public也访问不到

#### **Restrictions on Access Modifiers**

1. A subclass itself can be less accessible than a base class, but not more

2. When overriding a base class function, accessibility must be identical on the overridden function.

   ![image-20200818215537077](D:\Desktop\Grocery\TyporaImageBackUp\image-20200818215537077.png)



### **Interfaces**

An interface is similar to a class, but it provides a **specification** rather than an **implementation** for its members. An interface is special in the following ways:

1. Interface members are all implicitly **abstract**.

2. Interface members are always implicitly **public** and cannot declare an access modifier.

3. A class (or struct) can implement multiple interfaces.

4. An interface can contain only methods, properties, events, and indexers, which noncoincidentally are precisely the members of a class that can be abstract.(cannot have a field,nor a default value for the property)

5. 实现接口的时候要用public

6. <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200818215717032.png" alt="image-20200818215717032" style="zoom:67%;" />

   > Implementing an interface means providing a public implementation for all its members，其中第一行相当于用子类的对象建了一个Interface(相当于父类）的引用（up cast），type是父类，**和类的继承不同**，此处可以直接使用Interface(相当于父类)type的引用访问子类(implement class)的成员。
   >
   > 和抽象类类似，如果父类有abstract方法，子类实现这个方法，从子类对象创建父类对象的引用，
   >
   > Implicitly cast 一个类到被这个类实现的interface, 可以访问实现过的那些成员（interface中定义的那些），不能访问类里不是实现interface的成员

#### **Extending an Interface**

![image-20200818220503648](D:\Desktop\Grocery\TyporaImageBackUp\image-20200818220503648.png)

> interface can derived from more than one other interface.   

#### Explicit Interface Implementation

1. explict implementation无需使用public关键字

   ![image-20200818220613234](D:\Desktop\Grocery\TyporaImageBackUp\image-20200818220613234.png)



#### **Implementing Interface Members Virtually**

1. 默认被implemented的Interface成员是sealed的，如果在implement的时候标记为virtual则可以被override

2. 如果在子类override了父类实现接口的方法，通过子类，父类，接口来访问这个方法都用子类的实现：

   ![image-20200818220728959](D:\Desktop\Grocery\TyporaImageBackUp\image-20200818220728959.png)

3. An explicitly implemented interface member cannot be marked virtual,nor can it be overridden in the usual manner. It can, however, be reimplemented.

#### **Reimplementing an Interface in a Subclass p115**

1. e.g.用了接口名加点的explicitly implement，所以不能标记为virtual，所以子类重新实现这个接口来变相的override

   ![image-20200818220921538](D:\Desktop\Grocery\TyporaImageBackUp\image-20200818220921538.png)

2. Calling the reimplemented member through the interface calls the subclass’s implementation:

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200818220952881.png" alt="image-20200818220952881" style="zoom: 67%;" />

3. 子类reimplement父类已经实现过的 interface, 无论父类实现这个接口的时候是否是virtual的，无论父类是不是显式实现的接口，实例化一个子类对象，把这个子类对象cast到接口调用的是子类的实现，把这个对象cast到父类还是父类的实现。

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200818221050994.png" alt="image-20200818221050994" style="zoom:67%;" />

   调用时：Calling the reimplemented member through the interface calls the subclass’s implementation

   <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200818221129545.png" alt="image-20200818221129545" style="zoom:67%;" />



##### **Alternatives to interface reimplementation** p116

#### **Interfaces and Boxing**

e.g.第一种情况为调用一个implicitly implemented member on a struct( not causes boxing)第二种情况为Converting a struct to an interface( causes boxing)

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200818221231900.png" alt="image-20200818221231900" style="zoom:67%;" />

#### Summary for Interface

**WRITING A CLASS VERSUS AN INTERFACE:p117**As a guideline: Use classes and subclasses for types that naturally share an implementation. Use interfaces for types that have independent implementations.

子类new了父类的方法(子类父类方法同名)，子类对象cast到父类时是父类的实现

父类是方法是virtual，子类方法override了父类的方法，子类对象cast到父类时调用的是子类的实现

父类方法是抽象方法，子类继承父类并实现(override)父类的抽象方法，子类对象cast到父类时调用的是子类的实现，父类不可被实例化.

"父类"是一个接口，子类实现这个接口里的方法，子类对象cast到接口的时候实现的是子类的实现。

### **Enums**