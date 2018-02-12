## boost库中thread多线程----线程中断

>线程不是在任意时刻都可以被中断的。如果将线程中函数中的sleep()睡眠等待去掉，那么即使在主线程中调用interrupt()线程也不会被中断。

> thread库预定义了若干个线程的中断点，只有当线程执行到中断点的时候才能被中断，一个线程可以拥有任意多个中断点。

thread库预定义了共9个中断点，它们都是函数，如下：

1. thread::join();
2. thread::timed_join();
3. condition_variable::wait();
4. condition_variable::timed_wait();
5. condition_variable_any::wait();
6. condition_variable_any::timed_wait();
7. thread::sleep();
8. this_thread::sleep();
9. this_thread::interruption_point()

这些中断点中的前8个都是某种形式的等待函数，表明线程在阻塞等待的时候可以被中断。

而最后一个位于子名字空间this_thread的interruption_point()则是一个特殊的中断点函数，
它并不等待，只是起到一个标签的作用，表示线程执行到这个函数所在的语句就可以被中断。

## 创建线程

首先看看boost::thread的构造函数吧，boost::thread有两个构造函数：

    （1）thread()：构造一个表示当前执行线程的线程对象； 
    （2）explicit thread(const boost::function0& threadfunc)

boost::function0可以简单看为：一个无返回(返回void)，无参数的函数。这里的函数也可以是类重载operator()构成的函数；
该构造函数传入的是函数对象而并非是函数指针，这样一个具有一般函数特性的类也能作为参数传入，在下面有例子。

第一种方式：最简单方法 

```c++
void hello() 
{ 
        std::cout << 
        "Hello world, I''m a thread!" 
        << std::endl; 
} 
  
int main(int argc, char* argv[]) 
{ 
        boost::thread thrd(&hello); 
        thrd.join(); 
        return 0; 
}
```
第二种方式：复杂类型对象作为参数来创建线程：

```c++
boost::mutex io_mutex; 
struct count 
{ 
        count(int id) : id(id) { } 
        
        void operator()() 
        { 
                for (int i = 0; i < 10; ++i) 
                { 
                        boost::mutex::scoped_lock 
                        lock(io_mutex); 
                        std::cout << id << ": " 
                        << i << std::endl; 
                } 
        } 
        
        int id; 
}; 
  
int main(int argc, char* argv[]) 
{ 
        boost::thread thrd1(count(1)); 
        boost::thread thrd2(count(2)); 
        thrd1.join(); 
        thrd2.join(); 
        return 0; 
}

```

第三种方式：在类内部创建线程； 

```c++
class HelloWorld
{
public:
 static void hello()
 {
      std::cout <<
      "Hello world, I''m a thread!"
      << std::endl;
 }
 static void start()
 {
  
  boost::thread thrd( hello );
  thrd.join();
 }
 
}; 
int main(int argc, char* argv[])
{
 HelloWorld::start();
 
 return 0;
} 
在这里start()和hello()方法都必须是static方法。

```
（2）如果要求start()和hello()方法不能是静态方法则采用下面的方法创建线程：

```
class HelloWorld
{
public:
 void hello()
 {
    std::cout <<
    "Hello world, I''m a thread!"
    << std::endl;
 }
 void start()
 {
  boost::function0< void> f =  boost::bind(&HelloWorld::hello,this);
  boost::thread thrd( f );
  thrd.join();
 }
 
}; 
int main(int argc, char* argv[])
{
 HelloWorld hello;
 hello.start();
 return 0;
}
```
第四种方法：用类内部函数在类外部创建线程；

```c++
class HelloWorld
{
public:
 void hello(const std::string& str)
 {
        std::cout < }
}; 
  
int main(int argc, char* argv[])
{ 
 HelloWorld obj;
 boost::thread thrd( boost::bind(&HelloWorld::hello,&obj,"Hello 
                               world, I''m a thread!" ) ) ;
 thrd.join();
 return 0;
}
```

>如果线程需要绑定的函数有参数则需要使用boost::bind。比如想使用 boost::thread创建一个线程来执行函数：void f(int i)，
如果这样写：boost::thread thrd(f)是不对的，因为thread构造函数声明接受的是一个没有参数且返回类型为void的型别，
而且不提供参数i的值f也无法运行，这时就可以写：boost::thread thrd(boost::bind(f,1))。涉及到有参函数的绑定问题基本上
都是boost::thread、boost::function、boost::bind结合起来使用。