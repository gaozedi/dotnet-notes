# Skill 4.1: Perform I/O operations

Any object that can work with a Stream can work with any of the objects that behave like a stream.

![image-20200710164105415](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710164105415.png)

### **FileStream **(use async method perfered)

新建了一个FileStream, 输出bytes, 再新建一个FileStream读取刚刚的输出。

![image-20200710164529112](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710164529112.png)

#### Control file use with FileMode and FileAccess

> The base Stream class provides properties that a program can use to determine the abilities of a given stream instance (whether a program can read, write, or seek on this stream).

- The FileMode enumeration is used in the constructor of a FileStream to indicate how the file is to be opened.

  ![image-20200710165055439](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710165055439.png)

- The FileAccess enumeration is used to indicate how the file is to be used. The following access types are available:

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200710165152185.png" alt="image-20200710165152185" style="zoom:67%;" />



####  IDispose and FileStream objects

![image-20200710165346086](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710165346086.png)

#### Work with text files

> **读取text file的short cut method.**

![image-20200710165507843](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710165507843.png)

### Chain streams together

> **The Stream class has a constructor that will accept another stream as a parameter, allowing the creation of chains of streams.**

> e.g.保存,读取压缩文件

![image-20200710165654709](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710165654709.png)

### File class

一个Helper class,包含静态方法copy a file, create a file, delete a file, move a file, open a file, read a file, and manage file security.

![image-20200710165958241](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710165958241.png)

### Handle stream exceptions

![image-20200710170148338](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710170148338.png)

### Files storage

- DriveInfo :获取磁盘信息

  ![image-20200710170410533](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710170410533.png)

  输出示例:

  ![image-20200710170431436](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710170431436.png)

- **FileInfo class**

  > 可以用FileInfo 类打开文件，读取写入移动重命名等，有些功能和File类重复，File类一般用来操作单个文件，FileInfo操作多个文件。

  ![image-20200710170826221](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710170826221.png)

  使用:

  ![image-20200710170911091](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710170911091.png)

  - 用FileInfo类GetFile(string searchPattern)搜索文件

    > searchPattern可以接受*号代表个数字符,?号代表任意单个字符, 如下使用递归查找目录里文件

    <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200710172705851.png" alt="image-20200710172705851" style="zoom:67%;" />

    

- **Directory and DirectoryInfo classes**

  > **As with files, there are two ways to work with directories: the Directory class and the DirectoryInfo class.**

  >  The **Directory** class is like the File class. It is a static class that provides methods that can enumerate the contents of directories and create and manipulate directories.

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200710171333455.png" alt="image-20200710171333455" style="zoom:80%;" />

  > An instance of the **DirectoryInfo** class describes the contents of one directory. The class also provides methods that can be used to cerate and manipulate directories. 下面的代码和上面的功能一样。

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200710171511009.png" alt="image-20200710171511009" style="zoom:60%;" />



### Files paths

- **relative path**

  > A relative path specifies the location of a file relative to the folder in which the program is presently running.
  >
  > **When expressing paths, the character “.” (period) has a special significance. A single period “.” means the current directory. A double period “..” means the directory above the present one.**The **@** character at the start of the string literal marks the string as a verbatim string. This means that any escape characters in the string will be ignored (backslash). 

  ![image-20200710171735790](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710171735790.png)

  程序运行的时候再Debug文件夹下，上面的代码路径如下

  ![image-20200710171903735](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710171903735.png)

  

- **Absolute Path**

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200710171945190.png" alt="image-20200710171945190" style="zoom:67%;" />

- Path 类

  > 可以用来执行如下操作

  ![image-20200710172136122](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710172136122.png)

  可能的输出结果

  ![image-20200710172156201](D:\Desktop\Grocery\TyporaImageBackUp\image-20200710172156201.png)

# Skill 4.2: Consume data

### Query a Database



e.g.

make a connection to a database, create an SQL query, and then execute this query on the database to read and print out information from the MusicTrack table.

![image-20200724205029125](D:\Desktop\Grocery\TyporaImageBackUp\image-20200724205029125.png)

###  Update data in a database

e.g.

![image-20200724205224856](D:\Desktop\Grocery\TyporaImageBackUp\image-20200724205224856.png)

#### SQL injection

**you should never construct SQL commands directly from user input. You must use parameterized SQL statements instead.**

![image-20200724205257318](D:\Desktop\Grocery\TyporaImageBackUp\image-20200724205257318.png)

### Asynchronous database access(perfered)

![image-20200724213821924](D:\Desktop\Grocery\TyporaImageBackUp\image-20200724213821924.png)

# Skill 4.3: LINQ

- **Basic**

  ![image-20200724214012091](D:\Desktop\Grocery\TyporaImageBackUp\image-20200724214012091.png)

- **LINQ projection(select operation)**

  ![image-20200724214748616](D:\Desktop\Grocery\TyporaImageBackUp\image-20200724214748616.png)

-  **Anonymous types**

  ![image-20200724214807319](D:\Desktop\Grocery\TyporaImageBackUp\image-20200724214807319.png)

- **LINQ join**

  e.g.

  ![image-20200725202034001](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725202034001.png)

  ![image-20200725202145104](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725202145104.png)

  > **The first query selects the artist with the name “Rob Miles.” The results of that query are joined to the second query that searches the musicTracks collection for tracks with an ArtistID property that matches that of the artist found by the first query.**

  

- **LINQ group**

  > **group the results of a query to create a summary output. For example, you may want to create a query to tell how many tracks there are by each artist in the music collection.**

  

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725202314820.png" alt="image-20200725202314820" style="zoom:67%;" />

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725202325380.png" alt="image-20200725202325380" style="zoom: 67%;" />

  输出:

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725202357620.png" alt="image-20200725202357620" style="zoom:67%;" />

  再用join

  <img src="D:\Desktop\Grocery\TyporaImageBackUp\image-20200725202601894.png" alt="image-20200725202601894" style="zoom:67%;" />

- **LINQ Take and skip**

  ![image-20200725202742458](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725202742458.png)

- **LINQ aggregate**

  上面group中的Count()也是aggregate方法之一，其他例如Sum():

  ![image-20200725203813286](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725203813286.png)

  x是group中的单个item.

  ![image-20200725203851651](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725203851651.png)

- **Select data by using anonymous types**

  > **sample programs have shown the use of anonymous types moving from creating values that summarize the contents of a source data object (for example extracting just the artist and title information from a MusicTrack value), to creating completely new types that contain data from the database and the results of aggregate operators.**

  ![image-20200725204242486](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725204242486.png)

- **method-based LINQ queries**

  ![image-20200725203927533](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725203927533.png)

  相当于

  ![image-20200725203941347](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725203941347.png)

  

### Force execution of a query

>  **The actual evaluation of a LINQ query normally only takes place when a program starts to extract results from the query. This is called deferred execution. If you want to force the execution of a query you can use the ToArray() method**

![image-20200725204345598](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725204345598.png)

**Note that in the case of this example the result will be an array of anonymous class instances.**

![image-20200725204412072](D:\Desktop\Grocery\TyporaImageBackUp\image-20200725204412072.png)

