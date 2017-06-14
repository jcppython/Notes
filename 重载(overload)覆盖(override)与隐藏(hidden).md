重载(overload)、覆盖(override)与隐藏(hidden)

#### 重载

-   相同的范围 (同一个类)
-   函数名相同
-   参数不同
-   virtual关键字可有可无



#### 覆盖&重写(子类覆盖父类)

-   不同的范围(分别位于子类和父类)
-   函数名相同
-   参数相同
-   父类函数必须有virtual关键字



#### 隐藏 (子类屏蔽父类同名函数)

-   不同的范围
-   函数名相同, 参数不同，无论有无virtual参数，父类同名函数被隐藏 (区别于重载)
-   函数名和参数均相同，若父类无virtual参数，则被隐藏 (区别于覆盖)
-   一种理解: 不能有不同范围的重载！



###### 关于隐藏

http://stackoverflow.com/questions/1628768/why-does-an-overridden-function-in-the-derived-class-hide-other-overloads-of-the



#### 关于覆盖&重写

```c++
class father
{
public:   
    // 以下三个write是重载
    int write(int value);
    int write(float value);
    virtual int write(double value)
}

class child0 : public father
{
public:
    // 覆盖&重新 (virtual可有可无)
    virtual int write(double value);
}

class child2 : public father
{
public:
    // 隐藏 (父类无virtual)
    virtual int write(int value);
}

class child3 : public father
{
public:
    // 隐藏
    // 此时, 父类的三个方法全部被隐藏
    int write(char* buf, size_t sz);
}
```











