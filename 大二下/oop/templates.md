# Templates
模板是泛型编程的基础，泛型编程即以一种独立于任何特定类型的方式编写代码。
模板是创建泛型类或函数的蓝图或公式。库容器，比如迭代器和算法，都是泛型编程的例子，它们都使用了模板的概念。

## 函数模板
模板函数定义的一般形式如下所示：
```cpp
template <typename type> ret-type func-name(parameter list)
{
   // 函数的主体
}// type 是函数所使用的数据类型的占位符名称，可在函数定义中使用。
```
例如·
```cpp
template <typename T> int compare(T const&a, T const& b){
    if(a>b) return 1;
    if(a<b) return -1;
    return 0;
}//模版定义：以关键字template开始，后跟一个模板参数列表，这是一个逗号分隔的一个或多个模板参数的列表，用小于号（<）和大于号(>)包围起来。

实例化函数模板：当我们调用一个函数模板时，编译器通常会根据函数传递的实参来推测模板实参。

cout<<compare(1,2)<<endl;

实参为int型，编译器会推测实参为int，并将它绑定到模板参数T上。编译器用推断出的模板参数来为我们实例化，这些编译器生成的版本通常被称为模板的实例。

模板关键字可以为typename或class
---------
#include <iostream>
#include <string>
 
using namespace std;
 
template <typename T>
inline T const& Max (T const& a, T const& b) 
{ 
    return a < b ? b:a; 
} 
int main ()
{
 
    int i = 39;
    int j = 20;
    cout << "Max(i, j): " << Max(i, j) << endl; 
 
    double f1 = 13.5; 
    double f2 = 20.7; 
    cout << "Max(f1, f2): " << Max(f1, f2) << endl; 
 
    string s1 = "Hello"; 
    string s2 = "World"; 
    cout << "Max(s1, s2): " << Max(s1, s2) << endl; 
 
    return 0;
}
```
输出：
```bash
Max(i, j): 39
Max(f1, f2): 20.7
Max(s1, s2): World
```


## 类模板

如函数模板
```cpp
template <class type> class class-name {
.
}
```
在这里，type 是占位符类型名称，可以在类被实例化的时候进行指定。您可以使用一个逗号分隔的列表来定义多个泛型数据类型
```cpp
#include <iostream>
#include <vector>
#include <cstdlib>
#include <string>
#include <stdexcept>
 
using namespace std;
 
template <class T>
class Stack { 
  private: 
    vector<T> elems;     // 元素 
 
  public: 
    void push(T const&);  // 入栈
    void pop();               // 出栈
    T top() const;            // 返回栈顶元素
    bool empty() const{       // 如果为空则返回真。
        return elems.empty(); 
    } 
}; 
 
template <class T>
void Stack<T>::push (T const& elem) 
{ 
    // 追加传入元素的副本
    elems.push_back(elem);    
} 
 
template <class T>
void Stack<T>::pop () 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::pop(): empty stack"); 
    }
    // 删除最后一个元素
    elems.pop_back();         
} 
 
template <class T>
T Stack<T>::top () const 
{ 
    if (elems.empty()) { 
        throw out_of_range("Stack<>::top(): empty stack"); 
    }
    // 返回最后一个元素的副本 
    return elems.back();      
} 
 
int main() 
{ 
    try { 
        Stack<int>         intStack;  // int 类型的栈 
        Stack<string> stringStack;    // string 类型的栈 
 
        // 操作 int 类型的栈 
        intStack.push(7); 
        cout << intStack.top() <<endl; 
 
        // 操作 string 类型的栈 
        stringStack.push("hello"); 
        cout << stringStack.top() << std::endl; 
        stringStack.pop(); 
        stringStack.pop(); 
    } 
    catch (exception const& ex) { 
        cerr << "Exception: " << ex.what() <<endl; 
        return -1;
    } 
}
```
输出：
```bash
7
hello
Exception: Stack<>::pop(): empty stack
```

## 非类型模板参数
非类型模板参数（Non-type Template Parameters）：非类型模板参数允许我们在模板中使用常量值作为参数。它们用于在模板定义中**指定一个常量值**，**而不是一个数据类型**。非类型模板参数可以是整数、枚举、指针或引用类型。在模板参数列表中，我们使用一个特定的类型来定义非类型模板参数。例如：
```cpp
template<int N>
int multiplyByN(int value) {
    return value * N;
}
```
在上面的例子中，N是一个非类型模板参数，它表示一个整数常量值。在函数体内，我们可以将N用作常量值来执行相应的计算。

```cpp
template <class T,int bounds = 100> 
class FixedVector {
public:
  FixedVector(); 
  T& operator[](int);
private: 
  T elements[bounds];// fixed-size array!
};
```
bounds是一个非类型模板参数，它的默认值是100。这个模板类将创建一个固定大小的数组，大小由bounds参数指定。

**使用：**
```cpp
FixedVector<int,50> v1;
FixedVector<int,10*5> v2;
FixedVector<int> v3;// ->FixedVector<int, 100>
```