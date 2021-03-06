#  cpp 关键字解释
## explicit

 `explicit` 关键字用来修饰类的构造函数，表明该构造函数是显式的。
既然有"显式"那么必然就有"隐式"，那么什么是显示而什么又是隐式的呢？

如果c++类的构造函数有一个参数，那么在编译的时候就会有一个缺省的转换操作：
将该构造函数对应数据类型的数据转换为该类对象，如下面所示：

```apple js
class MyClass  
{  
public:  
MyClass( int num );  
}  
//.  
MyClass obj = 10; //ok,convert int to MyClass 
```
>如果要避免这种自动转换的功能，我们该怎么做呢？
嘿嘿这就是关键字explicit的作用了，将类的构造函数声明为"显式"，
也就是在声明构造函数的时候前面添加上explicit即可，
这样就可以防止这种自动的转换操作

## Virtual

`Virtual`是C++ OO机制中很重要的一个关键字
virtual可用来定义类函数和应用到虚继承。  
友元函数 构造函数 static静态函数 不能用virtual关键字修饰；  
普通成员函数和析构函数 可以用virtual关键字修饰；


只要是学过C++的人都知道在类Base中加了Virtual关键字的函数就是虚拟函数
（例如下面例子中的函数print），于是在Base的派生类Derived中就可以通过
重写虚拟函数来实现对基类虚拟函数的覆盖。当基类Base的指针point指向派生
类Derived的对象时，对point的print函数的调用实际上是调用了Derived的
print函数而不是Base的print函数。这是面向对象中的多态性的体现.

```apple js
class Base  
{  
public:Base(){}  
public:  
       virtual void print(){cout<<"Base";}  
};  
   
class Derived:public Base  
{  
public:Derived(){}  
public:  
       void print(){cout<<"Derived";}  
};  
   
int main()  
{  
       Base *point=new Derived();  
       point->print();  
} 
```

### 重载和覆盖的区别

1. 重载的几个函数必须在同一个类中；
   覆盖的函数必须在有继承关系的不同的类中
2. 覆盖的几个函数必须函数名、参数、返回值都相同；
   重载的函数必须函数名相同，参数不同。参数不同的目的就是为了在函数调用的时候
   编译器能够通过参数来判断程序是在调用的哪个函数。
   这也就很自然地解释了为什么函数不能通过返回值不同来重载，
   因为程序在调用函数时很有可能不关心返回值，编译器就无法从代码中看出程序在调用的是哪个函数了。
3. 覆盖的函数前必须加关键字Virtual；
   重载和Virtual没有任何瓜葛，加不加都不影响重载的运作。
   
### 纯虚函数

C++语言为我们提供了一种语法结构，通过它可以指明，一个虚拟函数只是提供了一个可被子类型改写的接口。但是，它本身并不能通过虚拟机制被调用。这就是纯虚拟函数（pure
virtual function）。 纯虚拟函数的声明如下所示：
```apple js
class Query {
public:
// 声明纯虚拟函数
virtual ostream& print( ostream&=cout ) const = 0;
// ...
};
```
这里函数声明后面紧跟赋值0。
包含（或继承）一个或多个纯虚拟函数的类被编译器识别为抽象基类。
试图创建一个抽象基类的独立类对象会导致编译时刻错误。（
类似地通过虚拟机制调用纯虚函数也是错误的）
```apple js
// Query 声明了纯虚拟函数, 我们不能创建独立的 Query 类对象  
// 正确: NameQuery 是 Query 的派生类  
Query *pq = new NameQuery( "Nostromo" );  
// 错误: new 表达式分配 Query 对象  
Query *pq2 = new Query();
```
## mutable

mutable 从字面意思上来说，是「可变的」之意。

mutable 是用来修饰一个 const 示例的部分可变的数据成员的。
如果要说得更清晰一点，就是说 mutable 的出现，
将 C++ 中的 const 的概念分成了两种。

* 二进制层面的 const，也就是「绝对的」常量，在任何情况下都不可修改（除非用 const_cast）。
* 引入 mutable 之后，C++ 可以有逻辑层面的 const，也就是对一个常量实例来说，从外部观察，
它是常量而不可修改；但是内部可以有非常量的状态。
>当然，所谓的「逻辑 const」，在 C++ 标准中并没有这一称呼。这只是为了方便理解，而创造出来的名词。

 在 C++ 中，mutable 是为了突破 const 的限制而设置的。被 mutable 修饰的变量，将永远处于可变的状态，即使在一个 const 函数中，甚至结构体变量或者类对象为 const，其 mutable 成员也可以被修改。
 
 ## typedef
 
 * 定义一种类型的别名，而不只是简单的宏替换。可以用作同时声明指针型的多个对象。
 * 为复杂的声明定义一个新的简单的别名。方法是：在原来的声明里逐步用别名替换一部分复杂声明，
 如此循环，把带变量名的部分留到最后替换，得到的就是原声明的最简化版
 * 用typedef来定义与平台无关的类型。
 
 ### 两大陷阱
 
 1. 记住，typedef是定义了一种类型的新别名，不同于宏，它不是简单的字符串替换。
 
 先定义：
 ```apple js
 typedef char* PSTR;
```

 然后：
 ```apple js
 int mystrcmp(const PSTR, const PSTR);
```

 `const PSTR`实际上相当于`const char*`吗？不是的，它实际上相当于`char* const`。
 原因在于const给予了整个指针本身以常量性，也就是形成了常量指针 `char* const`。
 **简单来说，记住当const和typedef一起出现时，typedef不会是简单的字符串替换就行。**
 
 **###############################**
 
 2. typedef在语法上是一个存储类的关键字（如auto、extern、mutable、static、register等一样），虽然它并不真正影响对象的存储特性，如：
 typedef static int INT2; //不可行
 编译将失败，会提示“指定了一个以上的存储类”。
 
 ## inline（内联函数）
 
 > 关于调用函数的缺点：函数调用前要先保存寄存器，
 并在返回时恢复；复制实参；程序还必须转向一个新位置执行。
 
 内联函数的作用主要就是使用在一些短小而使用非常频繁的函数中，为了减少函数调用的开销，
 为了避免使用宏（在c++中，宏是不建议使用的）。比如内联函数
```
inline int  func(int x)
{
    return x*x;
} 
```
在调用的时候

```
cout<<func(x)<<endl
```
，在编译时将被展开为：
```apple js
cout<<(x*x)<<endl;
```
### 内联函数相对于宏的区别和优点

>从上面的分析中，可以看出，内联函数在表现形式上与宏很类似。但是内联函数和宏之间的区别很明显。
宏是在预处理时进行的机械替换，内联是在编译时进行的。内联函数是真正的函数，只是在调用时，没有调用开销，像宏一样进行展开。
内联函数会进行参数匹配检查，相对于带参数的宏有很好的优点，避免了处理宏的一些问题。

### 内联函数注意事项

1. 内联函数一定会内联展开吗？答案是否定的。对于内联函数，程序只是提供了一个“内联建议”，即建议编译器把函数用内联展开，但是真正是否内联，
是由编译器决定的，对于函数体过大的函数，编译器一般不会内联，即使制定为内联函数。
2. 在内联函数内部，不允许用循环语句和开关语句（if或switch）。内联函数内部有循环和开关，也不会出错，但是编译器会把它当做非内联函数的。
3. 关键字inline必须与函数定义体放在一起才能使函数真正内联，仅把inline放在函数声明的前面不起任何作用。因为inline是一种用于实现的关键字，
不是一种用于声明的关键字。内联函数的声明是不需要加inline关键字的，内联函数的定义是必须加inline的（除了类的定义部分的默认内联函数），
尽管很多书声明定义都加了，要注意理解声明和定义的区别。**内联函数应该在头文件中定义；在头文件中加入或修改内联函数时，使用了该头文件的所有源文件都必须重新编译** 
4. 在一个文件中定义的内联函数不能在另一个文件中使用。它们通常放在头文件中共享。
5. 内联函数的定义必须在第一次调用之前。注意，这里是定义之前，不仅仅是声明之前。对于普通函数，可以在调用之前声明，
调用代码之后具体定义（实现函数），但是内联函数要实现内联，必须先定义再调用，否则编译器会把在定义之前调用的内联函数当做普通函数进行调用

## friend(友元函数)

### 1. 为什么使用friend？

> 通常对于普通函数来说，要访问类的保护成员是不可能的，如果想这么做那么必须把类的成员都生命成为 public( 共用的) ，然而这做带来的问题遍是
任何外部函数都可以毫无约束的访问它操作它；另一种方法是利用 C++ 的 friend 修饰符，可以让一些你设定的函数能够对这些私有或保护数据进行操作。

### 2. 使用友元有哪些缺点？
> 使用友元的同时也破坏了类的封装特性，这即是友元最大的缺点。当对外声明为友元后，你的所有细节全部都暴露给了对方。
  就好像你告诉你朋友你很有钱这个密秘，进而又把你有多少钱，多少古董，多少家产，多少小妾等等所有的家底全给他说了
  
### 3.总结

>简单的说就是：声明一个友元函数或者是友元类，就是要把自己完全暴露给对方。

## operator 运算符重载

```c++
//! equality test
bool operator == (const Coin &a, const Coin &b) {
    // Empty Coin objects are always equal.
    if (a.IsSpent() && b.IsSpent()) return true;
    return a.fCoinBase == b.fCoinBase &&
           a.nHeight == b.nHeight &&
           a.out == b.out;
}
```
operator 后面的 == 号就是被重载过的运算符，他所表达的意思就不是自己原有符号的意思，而是重载过后所代表的意思。

## extern 的详解

>extern是C/C++语言中表明函数和全局变量作用范围（可见性）的关键字。它告诉编译器，其声明的函数和变量可以在本模块或其它模块中使用,
extern可以置于变量或者函数前，以标示变量或者函数的定义在别的文件中，提示编译器遇到此变量和函数时在其他模块中寻找其定义。另外，extern也可用来进行链接指定。


### cpp生成可执行代码的全过程要经过几个步骤，依次是：

1. 预处理：预处理器（preprocessor）是一个简单的程序，它用程序员（利用预处理器指令）定义好的模式代替源代码中的模式
（删除注释、包含其他文件以及执行宏），预处理后生成中间文件.i（文本）。
2. 编译、优化：接下来对于.i文件进行语法分析。编译器把源代码分解成小的单元并把它们按树形结构组织起来。
表达式中“A + B”中的“A”、“+”和“B”就是语法分析树的叶子节点。语法分析树建立后有时会根据用户定义，使用全局优化器（global optimizer）
来生成更短、更快的代码。优代完成之后，还需要使用代码生成器（code generator）遍历语法分析树，把树的每个节点转化成汇编语言，
这个期间保存为中间文件.s（汇编语言 文本）。之后根据用户定义可以使用窥孔优化器（peephole optimizer）从相邻一段代码中查找冗余汇编语句。
3. 汇编： 接下来使用汇编器将汇编源文件翻译成对应的机器指令，而且还写入一些东西与机器指令打包成可重新定位目标程序格式的文件，
生成中间文件.o（目标文件 二进制）。
4. 链接：最后使用连接器（linker）把一组目标模块连接成为一个可执行程序（最终文件.exe），简单的讲，把目标的库文件和所需要的引用的静、
动态链接库进行链接，即需要把其他静态库合成到可执行文件中，转换相应的符号引用为地址，然后确保所引用的其他动态链接库的符号存在。
此外，链接器还要完成程序中各目标文件的地址空间的组织，这可能设计重定位工作。大多数现代操作系统都提供静态链接和动态链接两种形式。

**extern的原理很简单，就是告诉编译器：“你现在编译的文件中，有一个标识符虽然没有在本文件中定义，但是它是在别的文件中定义的全局变量，你可以使用。**

#### 使用extern和包含头文件来引用函数有什么区别呢？
extern的引用方式比包含头文件要简洁得多！extern的使用方法是直接了当的，想引用哪个函数就用extern声明哪个函数。
这大概是KISS原则（keep it simple,stupid）的一种体现吧！这样做的一个明显的好处是，会加速程序的编译（确切的说是预处理）的过程，
节省时间。在大型C程序编译过程中，这种差异是非常明显的。

## constexpr
>constexpr是C++11中新增的关键字，其语义是“常量表达式”，也就是在编译期可求值的表达式。
 
>constexpr函数要求所定义的函数足够简单以使得编译时就可以计算其结果

constexpr还能修饰类的构造函数，即保证传递给该构造函数的所有参数都是constexpr，那么产生的对象的所有成员都是constexpr， 
该对象也是constexpr对象了，可用于只使用constexpr的场合。 


**注意**  
constexpr构造函数的函数体必须为空，所有成员变量的初始化都放到初始化列表中。

### constexpr好处：
1. 是一种很强的约束，更好地保证程序的正确语义不被破坏。
2. 编译器可以在编译期对constexpr的代码进行非常大的优化，比如将用到的constexpr表达式都直接替换成最终结果等。
3. 相比宏来说，没有额外的开销，但更安全可靠。

### C++ const 和 constexpr 的区别呢？
* constexpr表示这玩意儿在编译期就可以算出来（前提是为了算出它所依赖的东西也是在编译期可以算出来的）。
* 而const只保证了运行时不直接被修改（但这个东西仍然可能是个动态变量）