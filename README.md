# C++ 
参考视频教程：[C++现代实用教程:基础主线](https://www.bilibili.com/video/BV1S54y1Z7Wc) 和 [C++现代实用教程:面向对象基础](https://www.bilibili.com/video/BV1eg411Q7nG)

## C++ 基础主线
### 1 基础与语法（输入输出、变量常量、位运算）
- 有哪两种打印方式？
  - `cout`(头文件iostream, 还包括`cin/endl/cerr/clog`) 和 `printf`
  - `cout` 实质是调用了对象的运算符函数`operator<<`
  - `endl` 是一个操纵符，除了换行还有刷新缓冲区的功能
- 如何读取命令行输入参数?
  - 如下，`argc`表示输入的参数数目,`argv`为输入的参数字符串数组。
  ```cpp
  int main(int argc, char* argv[]){
    if(argc != 1)
        for(size_t i = 0; i < argc; i++)
            cout << "arg" << i << " : " << argv[i] << endl;
  }
  ```
  - `./main a b c` 的第0个输入参数是运行的程序是`./main`。
- 变量和常量如何赋值？
  - 变量有两种写法：`int a = 10;`或`int a {10};`
  - 有两种常量：
    - `const int a = 10;`
    - `#define A 10` （也叫预定义，只是简单替换）
- `sizeof()`如何使用？
  - `sizeof()` 可以接数据类型或对象  
    ```cpp
    int x{0}; 
    cout << sizeif(x) << ' ' << sizeof(int);
    ```
- 如何使用`auto`时制定类型？
  - `auto x{1ll};`
  - `auto x{1l};` 注意一个`l`表示`Long Double`
- 位运算符使用有何注意事项？
  - 位运算符也是一种操作符，注意加括号
    ```cpp
    int a{3}, b{5};
    cout << (a & b) << ' ' << (a | b) << endl;
    ```
### 2 控制流
- c++有哪些选择语句？
  - `if-else`、`switch`、三元表达式
- C++有哪些循环语句？
  - `for`、`while`
- `cmath`头文件常用函数有哪些？
  - `abs` 取绝对值, `max`/`min` 比大小
  - `floor` 向下取整, `round` 四舍五入
  - `pow`/`sqrt` 乘方开方 `exp`/`log`
- 如何生成随机数？
  ```cpp
  srand(time(NULL)); // 根据时间生成随机数种子
  cout << rand() % 10 << endl; // 随机生成 [0, 9]
  ```
  > 并不需要 `include`

### 3 函数
- 什么是实参？什么是形参？
  ```cpp
  int func(int x) {...}
  cout << func(a) << endl;
  ```
  - `a` 是实参，`x`是形参
  - 形参在函数调用时分配内存，调用结束时释放内存
- 什么是函数的值传递？什么是指针传递？什么是引用传递？
  ```cpp
  int pass_by_value(int);
  int pass_by_point(int *const);
  int pass_by_reference(int &);
  int main(int argc, char *argv[]) {
    int x{0};
    cout << pass_by_value(x) << endl;
    cout << pass_by_point(&x) << endl;
    cout << pass_by_reference(x) << endl;
  }
  int pass_by_value(int x) {return x + 10;}
  int pass_by_point(int *const x) {return *x + 10;}
  int pass_by_reference(int &x) {return x + 10;}
  ```
  - 以上三种传递方式实现的功能是一致的，推荐使用**引用传递**。
- 什么是函数的重载？
  - 同一作用域内有一组函数名相同，但是参数列表不同的函数。（C不可以，C++可以）
- 什么是lambda函数？什么是值捕获？什么是引用捕获？
  - lambda函数又称匿名函数
  - `[]`中的是捕获列表，可以捕获lambda函数所在的局部变量
  - `()`中的是参数列表，是调用lambda函数时给出的
  - 值捕获：`[a](){}`(部分参数) / `[=](){}`(所有参数)
  - 引用捕获：`[&a](){}`(部分参数) / `[&](){}`(所有参数)
  ```cpp
  int x{10};
  auto func = [=](int a, int b){
  // 也可以写成 auto func = [x](int a, int b)
      return a + b + x;
  };
  cout << func(10, 20) << endl;
  ```
### 4 引用和指针
- 堆和栈的区别？
  - 栈：
    - 局部变量、函数调用都在栈上
    - 生命周期由scope（大括号）决定
  - 堆：
    - `new` / `delete`
    - 运行时分配，生命周期由开发者决定，与scope无关
- 指针有什么哪些常见错误？
  - 内存泄露：
    - 忘记`delete`
    - 没有`delete`，就重新指向新的变量
    - `delete`之后没有手动置为空，并访问
    ```cpp
    int a{1};
    int *p1 = &a; // 栈上的指针
    int *p2 {new int{2}}; // 堆上的指针
    delete p2;
    p2 = nullptr; // delete 之后需要手动置空
    cout << *p2 << endl; // 否则这行会正常运行
    ```
  - 指针**不能**指向不同类型
- 引用和指针有哪些区别？
  - 指针可以改变指向，引用不行
  - 指针可以不初始化，引用必须初始化，指针不安全
  - 指针需要解引用，引用不需要
- 指针与`const`有哪些组合？引用与`const`有哪些组合？
  - 指针（3种）
    - `int *const` 不可改变指向（相当于**引用**）
    - `const int*` 指向的值是const的，不能改变指向的值
    - `const int *const` 既不能改变指向，也不可以改变值
  - 引用（1种）
    - `const int&` 相当于 `const int *const`
  > 注意 `const` 的位置，是相对于`*`的。就是说，`const int*`和`int const*`是一个东西！

- 什么是函数指针？什么是回调callback？
  ```cpp
  char encoder(const char &c){
      return c - 10;
  }
  char decoder(const char &c){
      return c + 10;
  }
  void change(string& s, char(*func)(const char &c)) {
      for(auto &c : s) {
          c = func(c);
      }
  }
  int main(int argc, char *argv[]) {
      string s{"hello_world!"};
      cout << s << endl;
      change(s, encoder);
      cout << s << endl;
      change(s, decoder);
      cout << s << endl;
  }
  // 输出：
  // hello_world!
  // ^[bbeUmehbZx17
  // hello_world!
  ```
  例子中，形参`char(*func)(const char &c)`是一个函数指针，`char`是返回值类型，`const char &`是输入值类型。`change`就是一个回调函数。
- 什么是左值右值？如何获取右值的引用？
  - 左值是可以获取地址的值，右值是临时的不能获取地址的值
  - 右值可以通过`&&`获取引用
  ```cpp
  int add(const int& a, const int& b) {
      return (a + b);
  }
  int&& x = add(1, 2);
  ```
### 5 字符串
- 如何判断一个字符是否是字母、数字、大小写？如何转换大小写？
  - 判断：`.isalnum()`字母或数字 /`.isalpha()`字母 /`.isdigit()`数字 /`.isblank()`空格
  - 大小写：`.isupper()` /`.islower()` /`.toupper()` /`.tolower()`
- 如何在字符串中查找子串？
  ```cpp
  string s{"test"};
  if(s.find("te") != string::npos) //不是s.end()
      cout << "find 'te'" << endl;
  ```
### 6 多文件编译

- 如何避免头文件重复include？
  - 第一种：
    ```cpp
    #ifndef TEST_H
    #define TEST_H
    ...
    #endif
    ```
  - 第二种：
    ```cpp
    #pragma once
    ```
- 多文件编译分为哪两步？
  - 编译和链接
    ```bash
    clang++ -c main.cpp test.cpp
    clang++ -o main main.o test.o
    ```
    等价于
    ```bash
    clang++ -o main main.cpp test.cpp
    ```
