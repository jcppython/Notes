# C++11中的std::function

类模版std::function是一种通用、多态的函数封装



## 牢记于心

『万众归一』

『放弃函数指针，它类型不安全』



各种可调用实体(普通函数、Lambda表达式、函数指针、以及其它函数对象等)都可封装，形成一个新的可调用的std::function对象，一切变得简单粗暴



## 使用注意的事项：

可调用实体转换为std::function对象需要遵守以下两条原则：

-   转换后的std::function对象的参数能转换为可调用实体的参数
-   可调用实体的返回值能转换为std::function对象的返回值

std::function对象最大的用处就是在实现函数回调，使用者需要注意，它不能被用来检查相等或者不相等，但是可以与NULL或者nullptr进行比较



# 来个实例

```c++
#include <functional>
#include <iostream>
using namespace std;

std::function< int(int)> Functional;

// 普通函数
int TestFunc(int a)
{
    return a;
}

// Lambda表达式
auto lambda = [](int a)->int{ return a; };

// 仿函数(functor)
class Functor
{
public:
    int operator()(int a)
    {
        return a;
    }
};

// 1.类成员函数
// 2.类静态函数
class TestClass
{
public:
    int ClassMember(int a) { return a; }
    static int StaticMember(int a) { return a; }
};

int main()
{
    // 普通函数
    Functional = TestFunc;
    int result = Functional(10);
    cout << "普通函数："<< result << endl;

    // Lambda表达式
    Functional = lambda;
    result = Functional(20);
    cout << "Lambda表达式："<< result << endl;

    // 仿函数
    Functor testFunctor;
    Functional = testFunctor;
    result = Functional(30);
    cout << "仿函数："<< result << endl;

    // 类成员函数
    TestClass testObj;
    Functional = std::bind(&TestClass::ClassMember, testObj, std::placeholders::_1);
    result = Functional(40);
    cout << "类成员函数："<< result << endl;

    // 类静态函数
    Functional = TestClass::StaticMember;
    result = Functional(50);
    cout << "类静态函数："<< result << endl;

    return 0;
}
```



使用C++ class时，使用一般的函数指针做回调会很有局限性

-   仅能使用static函数做回调
-   仅能使用static成员变量



以上内容摘自: http://www.jellythink.com/archives/771