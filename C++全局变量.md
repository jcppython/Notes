### C++全局变量

全局变量 & 静态全局变量



#### 1. 单文件中的全局变量

所谓全局变量，即函数外定义的变量，全局变量的作用域**从定义位置开始到文件结尾，即只有文件作用域**

```c++
// 文件 test1.cpp

void fun1()
{ /*You can't access global_var*/ }

int global_var = 99; // 全局变量

void fun2()
{ /*You can access global_var*/ }
```



#### 2. 多个文件中的全局变量

需要引用其他文件内全局变量时，使用`extern`关键字声明即可

```c++
// 文件test2.cpp

extern int global_var; // 声明一个外部全局变量，链接阶段处理

void func1()
{ /*You can access global_var*/ }
```



#### 3. 将全局变量局限到单个文件

-   逻辑上，不希望它被别处使用
-   代码上，避免同其他文件同名全局变量冲突(链接时会出错)

```c++
// 文件test3.cpp
// 如果test3.cpp 和 test1.cpp 在同一个工程，链接阶段会保持，重定义
float global_var = 99.9;
```



让我们重写`test1.cpp`，此时，test2.cpp将无法获得`global_var`，`test3.cpp`也不会同它由冲突

```c++
// 重写 文件test1.cpp

void fun1()
{ /*You can't access global_var*/ }

static int global_var = 99; // 全局变量

void fun2()
{ /*You can access global_var*/ }
```



某种程度上，逻辑上的需要更重要 [另外，命名冲突，也可以用namespace解决]，如下例子

```c++
// Game 需要使用 map 记录 <队伍编号, 队伍得分>
// 下面的写法没有问题, 但是需要Game地方也将同时引用map，没必要的, map仅仅是用于实现Game, 成员含义并不强[这个例子也许不那么合适]
#include <map.h>
class Game {
public:
    void get_score(int team);
private:
    map<int, int> score;
};

// 使用全局变量
// Game.h
class Game {
public:
    void get_score(int team); // 如何实现是.cpp的需要考虑的事情
private:
    // map<int, int> score;
};

// Game.cpp
#include <map.h>
static map<int, int> score;
void Game::get_score()
{ /*...*/}
```



