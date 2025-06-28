# Copy Constructor
- 拷贝构造函数是一种特殊的构造函数，它在创建对象时，是使用同一类中之前创建的对象来初始化新创建的对象。
- 拷贝构造函数通常用于：
  - 通过使用另一个同类型的对象来初始化新创建的对象。
  - 复制对象把它作为参数传递给函数。
  - 复制对象，并从函数返回这个对象。

如果在类中没有定义拷贝构造函数，编译器会自行定义一个。如果类带有**指针变量**，并有动态内存分配，则它必须有一个拷贝构造函数。拷贝构造函数的最常见形式如下：
```cpp
classname::classname (const classname &obj) {
   // 构造函数的主体
}
```
在这里，**obj** 是一个对象引用，该对象是用于初始化另一个对象的。
构造函数是没有名字的，虽然可以Line a=Line();但不能Line a=Line::Line();这样名字永远无法找到。

事例：
```cpp
#include <iostream>
 
using namespace std;
 
class Line {
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
   private:
      int *ptr;
};
 
// 成员函数定义，包括构造函数
Line::Line(int len) {
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
Line::Line(const Line &obj) {
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
Line::~Line(void) {
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void ) {
    return *ptr;
}
void display(Line obj) {
   cout << "line 大小 : " << obj.getLength() <<endl;
}
// 程序的主函数
int main( ) {
   Line line(10);
   display(line);
   return 0;
}
```
执行后输出结果：
```bash
调用构造函数
调用拷贝构造函数并为指针 ptr 分配内存
line 大小 : 10
释放内存
释放内存
```
> **解释：**
display(line obj)使用了参数传递，需要复制一份参数obj。
传参方式是 按值传递（pass by value），这意味着：
实参 line 会被 复制一份 传给函数 display；
复制过程中会调用 Line 类的 拷贝构造函数；
函数 display 结束时，这份复制品会被销毁，因此调用其 析构函数；
最后，main 中的原始对象 line 被销毁时，也会调用其析构函数。
更清晰地解释执行流程：
Line line(10);
调用 普通构造函数，分配内存并赋值为 10；
display(line);
将 line 按值传入 display，于是调用 拷贝构造函数 复制一份；
在 display() 中调用 getLength() 输出大小；
display() 函数结束，复制对象被销毁，调用 析构函数；
main() 结束，原始对象 line 被销毁，再次调用 析构函数。

一个类缺少 Copy Constructor 时，编译器会自动生成一个。

**事例2**：
```cpp
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      int getLength( void );
      Line( int len );             // 简单的构造函数
      Line( const Line &obj);      // 拷贝构造函数
      ~Line();                     // 析构函数
 
   private:
      int *ptr;
};
 
// 成员函数定义，包括构造函数
Line::Line(int len)
{
    cout << "调用构造函数" << endl;
    // 为指针分配内存
    ptr = new int;
    *ptr = len;
}
 
Line::Line(const Line &obj)
{
    cout << "调用拷贝构造函数并为指针 ptr 分配内存" << endl;
    ptr = new int;
    *ptr = *obj.ptr; // 拷贝值
}
 
Line::~Line(void)
{
    cout << "释放内存" << endl;
    delete ptr;
}
int Line::getLength( void )
{
    return *ptr;
}
 
void display(Line obj)
{
   cout << "line 大小 : " << obj.getLength() <<endl;
}
 
// 程序的主函数
int main( )
{
   Line line1(10);
 
   Line line2 = line1; // 这里也调用了**拷贝构造函数**
 
   display(line1);
   display(line2);
 
   return 0;
}
```
执行后：
```bash
调用构造函数
调用拷贝构造函数并为指针 ptr 分配内存 //line2=line1
调用拷贝构造函数并为指针 ptr 分配内存 //dispaly(line1)传参创建副本
line 大小 : 10
释放内存 //dispaly(line1)结束后释放形参内存
调用拷贝构造函数并为指针 ptr 分配内存 //dispaly(line2)传参创建副本
line 大小 : 10
释放内存 //dispaly(line2)结束后释放形参内存
释放内存 //line2结束后释放内存
释放内存 //line1结束后释放内存
```
析构函数与构造函数顺序相反，先构造的后析构。

函数中的静态变量是从调用函数开始构造后一直存在至主程序结束析构。它在每次调用该函数时会使用，此时不用重新构造，直接使用即可。是同一个静态变量。
而局部变量存在时间只有在大括号内。

继承中：
1. 先调用基类的构造函数
2. 再调用子对象类（成员变量的构造函数）
3. 最后调用派生类的构造函数
4. 调用顺序与派生类构造函数冒号后面给出的初始化列表（Derived(): m1(), m2(), Base1(), Base2()）没有任何关系，按照继承的顺序和变量再类里面定义的顺序进行初始化。 先继承Base2，就先构造Base2。先定义m2，就先构造m2
[实例](https://blog.csdn.net/qaqwqaqwq/article/details/123129994)