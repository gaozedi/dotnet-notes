# **Skill 3.1: Validate application input**

## **Choose the appropriate data collection type**

- `Array`

  - e.g.

    ```csharp
    //migrate an array into a larger one.
    int[] data = {1,2,3,4};
    int temp = new int[5];
    //destination,position
    data.CopyTo(temp,0);
    data = temp;
    ```

  - fixed size

  - An array of value types (for example an array of integers) holds the values themselves within the array, whereas for an array of reference types (for example an array of objects) each element in the array holds a reference to the object.Array自己是引用类型。

  - When an array is created, each element in the array is initialized to the default value for that type. Numeric elements are initialized to 0, reference elements to null, and Boolean elements to false.

  -  **Multi-dimensional arrays**

    <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725205300824.png" alt="image-20200725205300824" style="zoom:67%;" />

  -  **Jagged arrays**

    <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725205338539.png" alt="image-20200725205338539" style="zoom: 50%;" />

    > 是一个二维数据，每一行的长度不等

- `ArrayList`

  - 保存的是item的object类型的引用，所以一个ArrayList可以存放不同类型的对象，但是使用的时候需要cast

    ```csharp
    class Program
        {
            static void Main(string[] args)
            {
                ArrayList arrayList = new ArrayList();
                arrayList.Add(new C1());
                arrayList.Add(new C2());
                foreach (var item in arrayList)
                {
                    Console.WriteLine(item.ToString());
                    //Temp.C1
                    //Temp.C2
                }
            }
        }
        class C1
        {
        }
        class C2
        {
        }
    ```

    

- `List` : use generic,the number of items being stored is not known in advance

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200726110815830.png" alt="image-20200726110815830" style="zoom:80%;" />

- `Queue`: FIFO

  - `Enqueue(some data);`

  - `Dequeue(some data);`

- `Stack`:LIFO

  - Push(some data);
  - Pop();

- `LinkedList<T>`:perform frequent insertions and deletions of data items

- `Dictionary<TKey,TValue>` 

  ![image-20200726110841891](D:\Desktop\Grocery\TyporaImageBackUp\image-20200726110841891.png)

  e.g.统计字数

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725205845491.png" alt="image-20200725205845491" style="zoom:67%;" />

  - Dictionary不带Sort方法，但是可以用LINQ排序

    <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725205947732.png" alt="image-20200725205947732" style="zoom:67%;" />

- `HashSet`

  - **A set is an unordered collection of items.**

  - **Each of the items in a set will be present only once.**

  - e.g.1

    ![image-20200726110932436](D:\Desktop\Grocery\TyporaImageBackUp\image-20200726110932436.png)

  - e.g.2

    ![image-20200725210756660](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725210756660.png)

    

- `StringCollection` and `StringList`: store only string

  

### Initialize a collection

**Initialize a collection**

<img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725211131063.png" alt="image-20200725211131063" style="zoom:80%;" />

### Custom collection

- **create a new type that implements the ICollection** **interface.**

- **use an existing collection class as the base (parent) class of a new collection type.**

  ![image-20200726111722376](D:\Desktop\Grocery\TyporaImageBackUp\image-20200726111722376.png)

## regular expression

- `replace()`: 

  ![image-20200620111020641](C:\Users\ggggg\AppData\Roaming\Typora\typora-user-images\image-20200620111020641.png)

  > output:   Rob,Mary,David,Jenny,Chris,Imogen,Rodney

- common regular signs:

  - `+`: one or more times

  - `.` :match any character 

    e.g. 

    ```csharp
    string input = "Rob Miles:My Way:120";
    string regexToMatch = ".+:.+:.+";
    if (Regex.IsMatch(input,regexToMacth))
        Console.WriteLine("Valide input")
    ```

    

  - `[ch-ch]`: match any character in that range, so the sequence` [0-9]` will match any digit, and the sequence` [0-9]+` will match one or more digits.

  - `^`:the start of a line

  - `$`: the end of a line

  - `|` :or 

    ![image-20200620112306612](C:\Users\ggggg\AppData\Roaming\Typora\typora-user-images\image-20200620112306612.png)

## Reading values

- `public static bool TryParse (string s, out int result);`

- `public static int ToInt32 (object value);`

- ![image-20200620113530590](C:\Users\ggggg\AppData\Roaming\Typora\typora-user-images\image-20200620113530590.png)

  

# **Skill 3.2: Perform symmetric and asymmetric encryption**

## **symmetric** encryption

- the key at each side of the conversation is exactly the same—the book

- **AES**: Any new systems requiring data encryption should use AES

  - e.g.![image-20200620113952443](C:\Users\ggggg\AppData\Roaming\Typora\typora-user-images\image-20200620113952443.png)

    ![image-20200620114028003](C:\Users\ggggg\AppData\Roaming\Typora\typora-user-images\image-20200620114028003.png)

    ![image-20200620114422909](C:\Users\ggggg\AppData\Roaming\Typora\typora-user-images\image-20200620114422909.png)

    

- **DES**: replaced by AES

- **RC2**: Insecure now

- **Rijndael**: AES is implemented as a subset of Rijndael

- **TripleDES**: The electronic payment industry makes use of TripleDES

## asymmetric encryption

- 用非对称加密来传递信息：公钥发给对方，对方用自己公钥加密的信息只有自己的私钥才能解密，中途的人只能得到加密后的信息。（自己的私钥用来解密）

- 用非对称加密来签名：自己用自己的私钥加密信息，对方（其他所有人）只有用自己的公钥才能解密，对方解密成功就知道信息一定来自自己。（自己的私钥用来加密）

  - 我发布文件给对方的同时，我也发布了我自己的公钥和用自己私钥加密后的"message digest"如checksum，对方可以用我的公钥解密message digest之后和自己计算的message digest比较，比较结果一致则说明文件没有被篡改。

    ![image-20200620135826017](C:\Users\ggggg\AppData\Roaming\Typora\typora-user-images\image-20200620135826017.png)

    

  - 公钥私钥保存问题：新建`RSACryptoServiceProvider`的时候可以告诉他公钥私钥保存在哪里：`CspParameters`类，`CspParameters.Flags = CspProviderFlags.UserMachineKeyStore`会用`Machine level key storage`，把密钥存放在`C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys`

- Digital signatures and certificates

  - 问题：Bob给Alice发送信息，Eve可以冒充Bob给Alice发送Eve自己的公钥私钥，或者Eve可以冒充Alice收Bob的信息，然后装作是Alice，用Bob给的公钥加密信息后给Bob回复。解决方法：大家去certification authority注册，向certification authority提交自己的信息

  - VS developer command prompt中 使用`makecert democert.cer`创建测试用的certificate ()

    本来是`new RSACryPtoServiceProvider()`，现在用`certificate.PrivateKey as RSACryPtoServiceProvider`

    ![image-20200620143754421](D:\Desktop\Grocery\TyporaImageBackUp\image-20200620143754421.png)



## **Implement the System.Security namespace**

## **Data integrity by hashing data**

- You “hash” a large lump of data to create a, hopefully small, lump of data that is distinctive for the large lump.A hashing algorithm will weight the values in the source data according to their positions in the data, so that these kinds of changes result in different hash codes being calculated.

-  所有的C#对象都有`GetHash()`方法，计算哈希值基于对象在内存中的位置，可以override这个方法。

  ![image-20200625214917395](D:\Desktop\Grocery\TyporaImageBackUp\image-20200625214917395.png)

  

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200625214946871.png" alt="image-20200625214946871" style="zoom:50%;" />

- 自带的`GetHash()`可以用来hash value of an object for indexing and searching，但是用来cryptographic applications you need a hashing algorithm that produces a larger hash value.

- 常见哈希算法：

  - MD5: produces a hash code that is 16 bytes (128) bits in size. It has been shown that it is possible to create different documents that both have the same MD5 hash code, 所以不安全了
  - SHA1:160bits，还是不安全
  - **SHA2: 224,256,384,512 bits, 安全**
    - ![image-20200625215505719](D:\Desktop\Grocery\TyporaImageBackUp\image-20200625215505719.png)
    - 输出：![image-20200625215516476](D:\Desktop\Grocery\TyporaImageBackUp\image-20200625215516476.png)

- **Encrypting streams**：有一个例子

# Skill 3.3: Manage assemblies

- You can regard an assembly as the output of a Visual Studio project. A Visual Studio solution can contain multiple projects.
- the contents of an assembly file are independent of the language that was used to create it.(语言无关性，可以是C# VB F#)，产生的MSIL也不是直接运行的，对应不同的平台把MSIL编译成不同的机器码执行(平台无关)![image-20200625220112303](D:\Desktop\Grocery\TyporaImageBackUp\image-20200625220112303.png)

### **Version assemblies**

- AssemblyInfo.cs 在.NET Core中变为MyProject.AssemblyInfo.cs

  

#### **Sign assemblies using strong names**



> However, strong-names are still required in applications in some rare situations, most of which are called out on this page: [Strong-Named Assemblies](https://docs.microsoft.com/en-us/dotnet/framework/app-domains/strong-named-assemblies).
>
> [how to sign assembly](https://docs.microsoft.com/en-us/dotnet/standard/assembly/sign-strong-name)

![image-20200625222223922](D:\Desktop\Grocery\TyporaImageBackUp\image-20200625222223922.png)



# Skill 3.4: Debug an application

### preprocessor compiler directives

- `#if`, `#define`

  ![image-20200707171640029](D:\Desktop\Grocery\TyporaImageBackUp\image-20200707171640029.png)

  - Conditional compilation symbols can also be defined in the properties of an application.

    > 现在有一个Symble `DIAGNOSTICS`定义了，如果要定义多个用semicolon分隔

    ![image-20200707171740809](D:\Desktop\Grocery\TyporaImageBackUp\image-20200707171740809.png)

- `#else`,`#elif`,`#endif`

  ![image-20200707172519731](D:\Desktop\Grocery\TyporaImageBackUp\image-20200707172519731.png)

- You can also use the `#undef ` directive to undefine any symbols that might have been defined outside your program source.

- Skill 2.5的`Conditional` Attribute

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200707172638232.png" alt="image-20200707172638232" style="zoom:67%;" />

- `Obsolete` directive

  > 第一个参数是Message, 第二个参数是是否产生warning

  ![image-20200707172735625](D:\Desktop\Grocery\TyporaImageBackUp\image-20200707172735625.png)

- `#warning` and `#error`

  `#warning`产生warning, `#error` 产生编译错误阻止程序继续执行。

  ![image-20200707173053618](D:\Desktop\Grocery\TyporaImageBackUp\image-20200707173053618.png)

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200707173042941.png" alt="image-20200707173042941" style="zoom:67%;" />

- **pragma directive**

  - 以下的方法本来有个warning因为异步方法没有await，但是现在阻止了这个方法产生warning.

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200707173336673.png" alt="image-20200707173336673" style="zoom:67%;" />

  - 只阻止一种类型的warning:

    <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200707173424723.png" alt="image-20200707173424723" style="zoom:50%;" />

  

- **Identify error positions with `#line`**

  - 重新指定了错误产生的行数：`#line 1 "kapow.ninja"`表示错误在第一行产生，错误的文件是kapow.ninja,

  - `#line default`表示恢复默认

    <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200707173832667.png" alt="image-20200707173832667" style="zoom: 67%;" />

-  **Hide code using `#line`**

  > debug中step debug的时候可以用来跳过自动产生的代码

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200707174047514.png" alt="image-20200707174047514" style="zoom:67%;" />

- **`[DebuggerStepThrough] attribute`**

  debug单步调试的时候会把整个方法作为一步

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200707174240094.png" alt="image-20200707174240094" style="zoom:50%;" />

  > 如果放在class上表示这个类里的所有方法都不会被`step through`

  

### Choose an appropriate build configuration

> VS自带两种**preset build configurations**: `Debug`和`Release`

Release版本不含有声明了但是没有用到的变量，如果在Release Build里面有断点，会触发**Just My Code** 功能。

> *Just My Code* is a Visual Studio debugging feature that automatically steps over calls to system, framework, and other non-user code. In the **Call Stack** window, Just My Code collapses these calls into **[External Code]** frames.

Release 版本中If a method body is very short the compiler may decide that the program runs faster if the method body was copied “inline” each time the method is called, rather than generating the instructions that manage a method call.

#### Specify symbol (.pdb) and source files in the Visual Studio debugger

https://docs.microsoft.com/en-us/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger?view=vs-2019

# Skill 3.5: Implement diagnostics in an application

- `Debug.WriteLine()`, `Debug.WriteLineIf()`输出错误到Output window

  > 如果不是Debug Compiled, 语句不会被输出。

  ![image-20200708162136111](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708162136111.png)

  ![image-20200708162414946](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708162414946.png)

- **Tarce Object**

  > 和上面的Debug一样，但是是在Release Build

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200708162539458.png" alt="image-20200708162539458" style="zoom:67%;" />





-  **Use assertions in Debug and Trace**

  ![image-20200708162648921](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708162648921.png)

  第一个Assert成功，第二个Assert 失败，产生如下窗口

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200708162900693.png" alt="image-20200708162900693" style="zoom:67%;" />

  

- **Use listeners to gather tracing information**

  > 默认情况下Debug和Trace输出到output window, 但是可以指定如下Listener

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200708163100460.png" alt="image-20200708163100460" style="zoom:67%;" />

  - e.g.

    ![image-20200708163114034](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708163114034.png)

  



### **Profiling applications**

-  **The StopWatch class**

  > **The StopWatch class provides `Start`, `Stop`, `Reset`, and `Restart` methods. The `Restart` method resets the stopwatch and starts it running again.**

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200708163421700.png" alt="image-20200708163421700" style="zoom:80%;" />









-------

执行到这行

相当于在那行设断点，再继续执行到那行。

![image-20200708165028495](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708165028495.png)

![image-20200708165049759](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708165049759.png)



写一个DebuggerDisplay属性，可以选择要显示object的什么Property

![image-20200708165343275](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708165343275.png)

![image-20200708165511238](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708165511238.png)



下一行要执行的语句由黄箭头表示，如果，可以**向上**拖动小箭头或者在执行过的语句选择如下，实现回过去执行**前面执行过的**语句，而不需要整个重新运行，但是有corrupt风险。

![image-20200708165842155](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708165842155.png)





Text Visualier

![image-20200708170130796](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708170130796.png)



![image-20200708170206531](D:\Desktop\Grocery\TyporaImageBackUp\image-20200708170206531.png)