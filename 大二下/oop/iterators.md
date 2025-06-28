# Iterators
迭代器是一种检查容器内元素并遍历元素的数据类型，通常用于对C++中各种容器内元素的访问，但不同的容器有不同的迭代器，初学者可以将迭代器理解为指针。

迭代器是一个对象，它提供了一种方法来遍历容器中的元素。迭代器可以被视为指向容器中元素的指针，但它比指针更加灵活和强大。迭代器可以用于访问、修改容器中的元素，并且可以与 STL 算法一起使用。

迭代器主要分为以下几类：
1. 输入迭代器（Input Iterator）：只能进行单次读取操作，不能进行写入操作。
2. 输出迭代器（Output Iterator）：只能进行单次写入操作，不能进行读取操作。
3. 正向迭代器（Forward Iterator）：可以进行读取和写入操作，并且可以向前移动。
4. 双向迭代器（Bidirectional Iterator）：除了可以进行正向迭代器的所有操作外，还可以向后移动。
5. 随机访问迭代器（Random Access Iterator）：除了可以进行双向迭代器的所有操作外，还可以进行随机访问，例如通过下标访问元素。

## 语法
```cpp
#include <iterator>

// 使用迭代器遍历容器
for (ContainerType::iterator it = container.begin(); it != container.end(); ++it) {
    // 访问元素 *it
}
```

**实例**
```cpp
#include <iostream>
#include <vector>
#include <iterator>

int main() {
    // 创建一个 vector 容器并初始化
    std::vector<int> vec = {1, 2, 3, 4, 5};

    // 使用迭代器遍历 vector
    for (std::vector<int>::iterator it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 使用 auto 关键字简化迭代器类型
    for (auto it = vec.begin(); it != vec.end(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl;

    // 使用 C++11 范围 for 循环
    for (int elem : vec) {
        std::cout << elem << " ";
    }
    std::cout << std::endl;

    return 0;
}
```