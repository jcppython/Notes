# 模板方法模式 Template Method



## 1. 要解决的问题

已知一个算法的框架(所需的关键步骤)，并确定这些步骤的执行顺序，但是某些步骤的具体实现是未知的，或者说某些步骤的实现与具体的环境相关。



**步骤框架已知，某些步骤的具体实现需要延后**



## 2. 模板方法模式

### 2.1 定义

**Template Method Pattern**: Definethe skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.

定义一个操作算法的骨架，而将一些步骤延迟到子类中，模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤



-   是基于继承的代码复用基本技术
-   模板方法模式的结构和用法也是面向对象设计的核心之一
-   在模板方法模式中，可以将相同的代码放在父类中，而将不同的方法实现放在不同的子类中




### 2.2 具体来说

-   基类抽象实现不变部分&公共部分, 将可变部分(行为)留给子类去实现
-   基类负责形式化地定义算法, 子类实现细节的处理
-   控制子类的扩展, 只在特定点调用“ hook”操作 ，这样就只允许在这些点进行扩展
-   模板方法模式导致一种反向的控制结构，这种结构有时被称为"好莱坞法则"，即"别找我们, 我们找你"。通过一个父类调用其子类的操作(而不是相反的子类调用父类)，通过对子类的扩展增加新的行为，符合“开闭原则”




### 2.3 两类方法

TemplateMethod是基类中的方法，该方法调用若干个 HookMethods 完成其任务

TemplateMethod和HookMethod是**模板方法模式**(TemplateMethod Pattern)的两部分



-   钩子方法的引入使得子类可以控制父类的行为。
-   最简单的钩子方法就是空方法, 也可以在钩子方法中定义一个默认的实现, 如果子类不覆盖钩子方法, 则执行父类的默认实现代码
-   比较复杂一点的钩子方法可以对其他方法进行约束，这种钩子方法通常返回一个boolean类型，即返回true或false，用来判断是否执行某一个基本方法。由子类来决定是否调用hook方法





## 3. 示例

```c++
class BankService {
public:
    // 抽象类
    virtual ~BankService() = 0;
    // Hook method
    virtual void service() = 0;
    virtual void score_worker(int score) = 0;

    // Template Method
    void service()
    {
       // 取号排队
       int number = get_number();
       // 办理具体业务
       specific_service();
       // 对银行工作人员进行评分
       score_worker();
    }

private:
    // 取号
    int get_number();

private:
    // 通用的私有成员
    int current_number;
};

class BankSaveMoney : public BankService {
public:
    virtual void service()
    {
        /*do your work*/
    }
    virtual void score_worker(int score)
    {
        /*do your work*/
    };
private:
    // 子类私有成员

};

class BankTakeMoney : public BankService {}
class BankRegisteAccount : public BankService {}
// ...
```



#### 4. 个人理解

提供一个 『在已定框架下, 修改其中某处动作的实际逻辑的机会』

-   hook方法

-   回调方法

-   std::function

-   函数指针 

-   多态，OO语言提供的一种运行时变化机制

    ​


## 参考:

http://blog.csdn.net/hguisu/article/details/7564039

http://c2.com/cgi/wiki?TemplateMethod