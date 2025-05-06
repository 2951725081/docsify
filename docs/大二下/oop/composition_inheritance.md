# Composition & Inheritance

## Composition
组合（Composition）是面向对象编程中类的一种关系，它表示一个类拥有另一个类的对象作为其成员。通过组合，一个类可以包含其他类的对象，从而实现复杂对象的创建和功能组合。在组合关系中，内嵌对象的生命周期与包含它们的对象的生命周期相同。

组合的主要特点
1. 内嵌对象初始化：在创建组合类对象时，首先会创建并初始化其内嵌对象。
2. 构造函数的顺序：组合类的构造函数首先调用内嵌对象的构造函数，调用顺序按照内嵌对象在类定义中的声明顺序，而不是在初始化列表中的顺序。然后执行组合类构造函数的主体。
3. 析构函数的顺序：组合类的析构函数会在销毁其内嵌对象之前执行完毕，内嵌对象会在逆序销毁。

例如：
```cpp
#include <iostream>
using namespace std;
 
// 组件类1
class Engine {
public:
    Engine() {
        cout << "Engine constructor called" << endl;
    }
    ~Engine() {
        cout << "Engine destructor called" << endl;
    }
};
 
// 组件类2
class Wheels {
public:
    Wheels() {
        cout << "Wheels constructor called" << endl;
    }
    ~Wheels() {
        cout << "Wheels destructor called" << endl;
    }
};
 
// 组合类：Car
class Car {
public:
    // 构造函数初始化列表含有内嵌对象
    Car() : engine(), wheels() {
        cout << "Car constructor called" << endl;
    }
    ~Car() {
        cout << "Car destructor called" << endl;
    }
private:
    // 声明组合关系的成员对象
    Engine engine;
    Wheels wheels;
};
 
int main() {
    // 创建组合类对象
    Car car;
    return 0;
}
 
/*
程序输出：
Engine constructor called
Wheels constructor called
Car constructor called
Car destructor called
Wheels destructor called
Engine destructor called
*/
```

1. 类定义
有2个组件类Engine和Wheels，以及组合类Car。Car类包含Engine和Wheels类的对象，并实现了组合关系。
2. 构造函数和析构函数
Car类包含一个Engine和一个Wheels对象，因此Car类拥有Engine和Wheels类的对象。在创建Car对象时，首先会创建并初始化其内嵌对象，然后执行Car类的构造函数。当销毁Car对象时，会先销毁其内嵌对象，然后执行Car类的析构函数。成员对象的析构顺序是成员变量声明的逆序。
3. 构造和析构过程
- 创建Car对象
    1. 创建Engine对象
    2. 创建Wheels对象
    3. 调用Car的构造函数

- 销毁Car对象
    1. 调用Car的析构函数
    2. 销毁Wheels对象
    3. 销毁Engine对象


## Inheritance
当你通过从另一个对象继承的方式来创建一个新的对象时，目的就应该是：使得新的对象有一个相对于原始对象更加特定的版本。 
当创建一个类时，不需要重新编写新的数据成员和成员函数，只需指定新建的类继承了一个已有的类的成员即可。这个已有的类称为基类，新建的类称为派生类。
继承代表了 is a 关系。例如，哺乳动物是动物，狗是哺乳动物，因此，狗是动物，等等。
例如：
```cpp
// 基类
class Animal {
    // eat() 函数
    // sleep() 函数
};
//派生类
class Dog : public Animal {
    // bark() 函数
};
```

### 基类和派生类
```cpp
class derived-class: access-specifier base-class
```
其中，访问修饰符 access-specifier 是 public、protected 或 private 其中的一个，base-class 是之前定义过的某个类的名称。如果未使用访问修饰符 access-specifier，则默认为 private。
例如：
```cpp
#include <iostream>
 
using namespace std;
 
// 基类
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 派生类
class Rectangle: public Shape
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   return 0;
}
```
运行后：
```cpp
Total area: 35
```
### 访问控制和继承
派生类可以访问基类中所有的非私有成员。因此基类成员如果不想被派生类的成员函数访问，则应在基类中声明为 private。

|访问|public|protected|private|
|--- | ---- |  -----| -----| 
|同一类|yes|yes|yes|
|派生类|yes|yes|no|
|外部类|yes|no|no|

### 继承类型
当一个类派生自基类，该基类可以被继承为 public、protected 或 private 几种类型。继承类型是通过上面讲解的访问修饰符 access-specifier 来指定的。

我们几乎不使用 protected 或 private 继承，通常使用 public 继承。当使用不同类型的继承时，遵循以下几个规则：
- 公有继承（public）：当一个类派生自公有基类时，基类的公有成员也是派生类的公有成员，基类的保护成员也是派生类的保护成员，基类的私有成员不能直接被派生类访问，但是可以通过调用基类的公有和保护成员来访问。
- 保护继承（protected）： 当一个类派生自保护基类时，基类的公有和保护成员将成为派生类的保护成员。
- 私有继承（private）：当一个类派生自私有基类时，基类的公有和保护成员将成为派生类的私有成员。

### 多继承
多继承即一个子类可以有多个父类，它继承了多个父类的特性。
```cpp
class <派生类名>:<继承方式1><基类名1>,<继承方式2><基类名2>,…
{
<派生类类体>
};
```
实例：
```cpp
#include <iostream>
 
using namespace std;
 
// 基类 Shape
class Shape 
{
   public:
      void setWidth(int w)
      {
         width = w;
      }
      void setHeight(int h)
      {
         height = h;
      }
   protected:
      int width;
      int height;
};
 
// 基类 PaintCost
class PaintCost 
{
   public:
      int getCost(int area)
      {
         return area * 70;
      }
};
 
// 派生类
class Rectangle: public Shape, public PaintCost
{
   public:
      int getArea()
      { 
         return (width * height); 
      }
};
 
int main(void)
{
   Rectangle Rect;
   int area;
 
   Rect.setWidth(5);
   Rect.setHeight(7);
 
   area = Rect.getArea();
   
   // 输出对象的面积
   cout << "Total area: " << Rect.getArea() << endl;
 
   // 输出总花费
   cout << "Total paint cost: $" << Rect.getCost(area) << endl;
 
   return 0;
}
```   

