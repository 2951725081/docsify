# CLASS
类用于指定对象的形式，是用户自定义的数据类型，封装了数据和函数的组合。类中数据称为成员变量，函数称为成员函数。类可看作一种模板，用来创建具有相同属性的多个对象。

## 类定义
![class](.\class.png "class")
```cpp
class class_name{
    Access_specifier:(访问修饰符：private/public/protected，默认为private);
        member_variable;
        member_function(){};
};//分号注意
/*
例子：
class Box
{
   public://在类的外部也可访问
      double length;   // 盒子的长度
      double breadth;  // 盒子的宽度
      double height;   // 盒子的高度
};
*/
```
在c++中，是用头文件与源文件来define class，class声明和成员函数原型都是在.h文件中，所有函数主体在.cpp源文件中。源文件与主文件中都只引.h头文件
运行程序：
1.编译 g++ xxx.cpp main.cpp  在运行a.exe
2.main.cpp 中引函数.cpp
**头文件**中包含：
- 类成员数据声明
- 类静态成员定义，声明
- 类成员函数声明
- 非类成员函数声明
- 常数定义（初始化），如const int a=5;
- 静态函数定义
- 类内联函数定义  

不包含：
- 所有非静态变量（不是类发数据成员，也不是常数）->放源文件中
- 默认命名空间不放在头文件中,using namespace std;等放.cpp中   

**标准化的头文件结构**
```cpp
#ifndef HEADER_H
#define HEADER_H

//all kinds of declarations here...

#endif //HEADER_H
```
一些小Tips:
1.一个类声明一个头文件
2.源文件对应头文件完成功能

*若要使用类似全局变量，在运行过程中不断改变其值，可考虑使用指针（函数中）*


**成员函数的定义**
1.在类定义时定义  
```cpp
class point{
    private:
        int xpos;
        int ypos;
    public:
        void set(int x,int y){
            xpos=x;
            ypos=y;
        }
        void print(){
            cout<<"xpos="<<xpos<<"ypos="<<ypos<<endl;
        }
};//成员函数中无需再将成员变量加到自变量里，默认已知
``` 
2.在类外定义成员函数
要用·作用域操作符::实现
```cpp
class point{
    private:
        int xpos;
        int ypos;
    public:
        void set(int x,int y);
        void print();
}

void point::set(int x,int y){
    xpos-x;
    ypos=y;
}
void point::print(){
    cout<<"xpos="<<xpos<<"ypos="<<ypos<<endl;
}
```
若选用这种方式定义成员函数，则声明是在.h中，定义是在.cpp中

---

## 作用域运算符用法

:: 
1.当存在具有相同名称的局部变量时，访问全局变量
```cpp
int x=0;
int main(){
    int x=10;
    cout<<"::x:"<<::x<<endl;
    cout<<"x:"<<x;
}
//output:
//::x:0
//x:10
``` 
2.在类外定义函数

具体见上

3.访问一个类的静态变量

4.如果有多个继承

5.对于命名空间

例如std::cout

6.在另一个类中引用一个类
    

---

## 访问修饰词
- **private**
默认值，在类外部不可访问，甚至不可查看。只有类和友元函数可以访问。
一般情况在私有区域定义数据，在共有区域定义相关函数。一般需要设置公有区域的获取函数和设置函数

- **public**
在类的外部可访问，不需要使用任何成员函数来设置和获取共有变量的值。

- **protected**
与私有成员相似，但是它可以在派生类（子类）中访问。
详情见后

## 定义对象
对象是根据类创建的，跟声明基本类型的变量一致。
```cpp
//例子
Box Box1;//声明一个对象，类型为Box
Box Box2;
```
对象Box1,Box2都有各自的数据成员

---

## 访问数据成员
可以直接用成员访问运算符.访问
```cpp
#include <iostream>
using namespace std;
class Box
{
   public:
      double length;   // 长度
      double breadth;  // 宽度
      double height;   // 高度
      // 成员函数声明
      double get(void);
      void set( double len, double bre, double hei );
};
// 成员函数定义
double Box::get(void)
{
    return length * breadth * height;
}
 
void Box::set( double len, double bre, double hei)
{
    length = len;
    breadth = bre;
    height = hei;
}
int main( )
{
   Box Box1;        // 声明 Box1，类型为 Box
   Box Box2;        // 声明 Box2，类型为 Box
   Box Box3;        // 声明 Box3，类型为 Box
   double volume = 0.0;     // 用于存储体积
 
   // box 1 详述
   Box1.height = 5.0; 
   Box1.length = 6.0; 
   Box1.breadth = 7.0;//成员变量为public，可用.访问，一般来说，成员变量为private，保护封装性
 
   // box 2 详述
   Box2.height = 10.0;
   Box2.length = 12.0;
   Box2.breadth = 13.0;
 
   // box 1 的体积
   volume = Box1.height * Box1.length * Box1.breadth;
   cout << "Box1 的体积：" << volume <<endl;
 
   // box 2 的体积
   volume = Box2.height * Box2.length * Box2.breadth;
   cout << "Box2 的体积：" << volume <<endl;
 
 
   // box 3 详述
   Box3.set(16.0, 8.0, 12.0); 
   volume = Box3.get(); 
   cout << "Box3 的体积：" << volume <<endl;
   return 0;
}
```
编辑运行后
```cpp
Box1 的体积：210
Box2 的体积：1560
Box3 的体积：1536
```
需要注意，私有成员(private)和受保护成员（protected）不可直接用.访问,而公共成员（public）可用.直接访问

---

## 构造函数（ctor/constructor）
一种特殊的成员函数，**会在每次创建类的新对象时执行**。它的名称与类的名称完全相同，并且不会返回任何类型，也不会返回void。它可用于为某些成员变量设置初始值。
**不带参数的**
实例1：
```cpp
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```
以上函数运行后结果：
```cpp
Object is being created
Length of line : 6
```
  
**带参数的ctor**
默认的ctor没有任何参数，但如果需要，构造函数也可带参数。这样在创建对象时会给对象赋初始值。
实例2：
```cpp
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line(double len);  // 这是构造函数
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line( double len)
{
    cout << "Object is being created, length = " << len << endl;
    length = len;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line(10.0);
 
   // 获取默认设置的长度
   cout << "Length of line : " << line.getLength() <<endl;
   // 再次设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```
输出结果：
```cpp
Object is being created, length =10
Length of line :10
LEngth of line :6
```
**使用初始化列表初始化字段**
使用初始化列表来初始化字段：
```cpp  
Line::Line(double len):length(len){
    cout << "Object is being created, length =" << length <<endl;
}
//以上语法等同于以下
Line::Line(double len){
    length = len;
    cout << "Object is being created, length =" << length <<endl;
}
```
假设由一个类C,具有多字段X,Y,Z,那么初始化列表可以这样写：
```cpp
C::C(double a,double b, double c): X(a),Y(b),Z(c){
    ...
}
```
## 析构函数（dtor/destructor）
类的析构函数是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。
析构函数的名称与类的名称完全相同，只是在前面加了发波浪号~作为前缀，它不会返回任何值，也不能带有任何参数。析构函数有助于在跳出程序（如关闭文件，释放内存）前释放资源。
实例：
```cpp
#include <iostream>
 
using namespace std;
 
class Line
{
   public:
      void setLength( double len );
      double getLength( void );
      Line();   // 这是构造函数声明
      ~Line();  // 这是析构函数声明
 
   private:
      double length;
};
 
// 成员函数定义，包括构造函数
Line::Line(void)
{
    cout << "Object is being created" << endl;
}
Line::~Line(void)
{
    cout << "Object is being deleted" << endl;
}
 
void Line::setLength( double len )
{
    length = len;
}
 
double Line::getLength( void )
{
    return length;
}
// 程序的主函数
int main( )
{
   Line line;
 
   // 设置长度
   line.setLength(6.0); 
   cout << "Length of line : " << line.getLength() <<endl;
 
   return 0;
}
```
程序执行后结果：
```cpp
Object is being created
Length of line :6
Object is being deleted
```
## this 指针
this指针是一个特殊的指针，它指向当前对象的实例。
每个对象都能通过this指针来访问自己的地址。
它是一个隐藏的指针。可以在类的成员函数的使用，可以用来指向调用对象。当一个对象的成员函数被调用时，编译器会隐式地传递该对象的地址作为 this 指针。友元函数没有 this 指针，因为友元不是类的成员，**只有成员函数才有 this 指针**。

实例：
```cpp
#include <iostream>
 
class MyClass {
private:
    int value;
 
public:
    void setValue(int value) {
        this->value = value;
    }
 
    void printValue() {
        std::cout << "Value: " << this->value << std::endl;
    }
};
 
int main() {
    MyClass obj;
    obj.setValue(42);
    obj.printValue();
 
    return 0;
}
//输出结果：
Value: 42
```
通过使用 this 指针，我们可以在成员函数中访问当前对象的成员变量，即使它们与函数参数或局部变量同名，这样可以**避免命名冲突，并确保我们访问的是正确的变量**。
```cpp
void Point::print()
➔ (can be regarded as)
void Point::print(Point *this)

Point a;
a.print();
➔ (can be regarded as)
Point::print(&a);
```

## const 关键字

1. const 修饰普通类型变量
```cpp
const int a = 10;
int b=a;//correct
a=8;//error
```
a 是 const 类型的，不能被修改。但可以给其他变量赋值。

2. const 修饰指针变量

有三种情况：
- 修饰指针变量指向的内容，则内容为不可变量
- 修饰指针变量本身，则指针变量本身不可变量
- 修饰指针和指针指向的内容，则指针变量和指针指向的内容均不可变量
```cpp
const int *a = 10;//指针变量指向的内容不可变量，因为const在*号左边，称为左定向

int b=8;
int *const p=&b;
*p=9;//correct
int d=7;
p=&d;//error
//const 指针p指向的内存地址不被改变，但内容可变，因为const在*号右边，称为右定向

int e=1;
const int *const p=&e;
//const指针p指向的内存地址和内容均不可变量，因为const在*号两边
```
3. const 参数传递和函数返回值

有三种情况：
- 值传递的const修饰传递，一般不需要const修饰，因为函数会自动产生临时变量赋值实参值。
```cpp
void func(const int a){
    cout<<a;
    //++a; 是错误的，a不能被改变
}
```

- 当const参数为指针时，可以防止指针被意外篡改。
```cpp
void func(int *const a){
    cout<<*a<<" ";
    *a=9;
}
int main(){
    int a=8;
    func(&a);
    cout<<a;//输出8 9,a为9
    return 0;
}
```

-  自定义类型的参数传递，需要临时对象复制参数，对于临时对象的构造，需要调用构造函数，比较浪费时间，因此我们采取 const 外加引用传递的方法。并且对于一般的 int、double 等内置类型，我们不采用引用的传递方式。

4. const 修饰类成员函数
const 修饰类成员函数，其目的是**防止成员函数修改被调用对象的值**，如果我们不想修改一个调用对象的值，所有的成员函数都应当声明为 const 成员函数。
*注意：const 关键字不能与 static 关键字同时使用，因为 static 关键字修饰静态成员函数，静态成员函数不含有 this 指针，即不能实例化，const 成员函数必须具体到某一实例。*
```cpp
#include<iostream>
 
using namespace std;
 
class Test
{
public:
    Test(){}
    Test(int _m):_cm(_m){}
    int get_cm()const
    {
       return _cm;
    }
 
private:
    int _cm;
};
 
 
 
void Cmf(const Test& _tt)
{
    cout<<_tt.get_cm();
}
 
int main(void)
{
    Test t(8);
    Cmf(t);
    system("pause");
    return 0;
}
```

## new/delelte

new/delete 是 C++ 中的内存管理机制，用于在堆上申请和释放内存。
new/delete都是运算符，不是库函数

格式：
new：
1. 类型指针 指针变量名= new 类型
2. 类型指针 指针变量名= new 类型(初始值)
3. 类型指针 指针变量名= new 类型[元素个数]
delete：
1. delete 指针变量名
2. delete[] 指针变量名
```cpp
	//申请空间
	int* ptr = new int;
	//申请空间并初始化
	int* ptr2 = new int(1);
	//申请连续的空间，空间大小为4*10=40
	int* arr = new int[10];//c++98不允许连续空间初始化

	//释放单个空间
	delete ptr;
	delete ptr2;

	//释放连续的多个空间
	delete[] arr;
```
new去申请对象会先申请对象的空间并调用对象的构造函数完成对象的初始化；delete会先去完成对象的资源清理，再将对象所占的空间释放掉。

但是要注意，如果没有默认构造函数，我们必须在new一个对象时后面要加小括号给予初始值进行初始化。没有默认构造函数，我们也不能申请连续的多个空间。
