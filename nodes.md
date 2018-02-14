## 一些琐碎的知识

### struct 和 class的区别

在struct中 成员默认是 public的；在class中，成员默认是private的。

### cpp模版编程

在我们编程中，我们会遇到有相同的参数列表，但是需要实现不同的功能需求，所以，这时候模版编程是一个不错的选择，
这个时候，使用cpp的`tag dispatch`它会在，参数列表的后面加一个类似标记位的参数，用来标示这个参数。

### << 符号
该符号指出了信息流动的路径，指向哪里代表流向谁。这里将 << 运算符重载，区别于位移 << 运算符。

> 对象是类的特定实例，而类定义了数据的存储和使用方式

### endl 控制符

在cpp中，endl代表换行，也就是重启一行。cpp同样支持 \n 进行换行操作。

### 变量声明
int aaa;
这条语句提供了两条信息，需要的内存，以及该内存单元的名称。

### cout 输出流
在cpp中，我们引入流cout关键字，表示输出流，它能够自动识别所需要输出的类型。而printf则不可以：

```c
printf("my name is : %s",name);
printf("my age is :%d",age);
``` 
正如上面代码所描述，如果你给定的类型是错误的，将会显示乱码，这也是printf的缺陷。

cout的另一个优点在于可以进行拼接。

```c++
cout<<"my name is " << name << "please help me ";
```
## 共用体

共用体是一种数据格式，可以存储不同的数据类型，举例如下：
```c++
union sum
{
    int int_val;
    long long_val;
    double double_val;
}

```
因此，sum有时是int，有时是long或double，根据需要的条件不同，会有不同的类型。
## 枚举
枚举，就是一个一个的列举，所以枚举类型即为能被列举的常量的一个集合。
             
枚举类型的定义写结构体的定义相似，其形式为：

```c++
enum 枚举名{ 
               标识符[=整型常数], 
               标识符[=整型常数], 

... 
               标识符[=整型常数], 

} 枚举变量;
```
如果枚举没有初始化, 即省掉"=整型常数"时, 则从第一个标识符开始,依次赋给标识符0, 1, 2, ...。
但当枚举中的某个成员赋值后, 其后的成员按依次 加1的规则确定其值。

举例如下：

```c++
#include <iostream>
using namespace std;

enum Day {Saturday, Sunday = 0, Monday, Tuesday, Wednesday,
Thursday, Friday}; //Saturday = 0 by default, Sunday = 0 as well
void Prnt (Day day)  // Print whether a day is a 'Weekend' or a "Weekday".
{
        if (day ==0) cout << "Weekend" << endl;
        else cout << "Weekday" << endl;
}

int main(){
        enum Fruit {apple, pear, orange, banana} frt1; // 'frt1' can be declarated here.
        
        // int apple; // error: redefinition of 'apple'
        
        typedef enum Fruit ShuiGuo; // In c++, 'enum' can be omitted.
        
        enum Fruit frt2 = apple; // In c++, 'enum' can be omitted.
        ShuiGuo frt3 = pear; // After type-declaration synonym, 'enum' can not exist here!
        
        frt1 = (Fruit) 0; // 'frt1' can be assigned with number by explicit cast.
        
        for (int i = apple; i <= banana; i++)
                switch (i)
                {
                   case apple: cout << "apple" << endl; break;
                   case pear: cout << "pear" << endl; break;
                   case orange: cout << "orange" << endl; break;
                   case banana: cout << "banana" << endl; break;
                   default: break;
                }
        
        // Print whether a day is a 'Weekend' or a "Weekday".
        Prnt (Saturday);
        Prnt (Sunday);
        Prnt (Monday);
        Prnt (Tuesday);
        Prnt (Wednesday);
        Prnt (Thursday);
        Prnt (Friday);
        
        
        return 0;
}


```