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

## 构造函数
一种特殊的成员函数，会在每次创建类的新对象时执行。

