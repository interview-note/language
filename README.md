# 现代 C++ 
参考视频教程：
- [C++现代实用教程:基础主线](https://www.bilibili.com/video/BV1S54y1Z7Wc) 
- [C++现代实用教程:面向对象基础](https://www.bilibili.com/video/BV1eg411Q7nG) 
- [C++现代实用教程:友元与继承](https://www.bilibili.com/video/BV1ZZ4y1e7zy)

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

## C++ 面向对象
### 1 概述
- 面向对象的优缺点？
  - 优点：易读性、扩展性好
  - 缺点：性能有一点损失
- 什么是面向对象的三大特性？
  - 继承：子类继承父类的属性和方法，C++支持多继承
  - 封装：封装成类，只开放特定的数据和方法
  - 多态：为不同的数据类型提供统一的接口，通过纯虚函数实现
  - 三大特性是设计模式的基石
### 2 类、对象、this
- 对象有哪两种生成方式？
  - 栈上 `A a;`
  - 堆上 `A* a = new A();`
- 对象（类）由什么构成？
  - 程序 = 算法 + 数据结构，类由属性和方法构成
  - 属性：
    - 对象相关属性
    - 类属性 -> 必须在主函数外初始化
  - 方法：
    - 构造函数
    - Getter/Setter 等
  ```cpp
  class A {
  private:
      static int cnt;         // 类属性
  public:
      int x, y;               // 对象属性
      A():x(0), y(0){++cnt;}; // 构造函数
      A(int x, int y) : x(x), y(y){++cnt;};
      ~A(){--cnt;};           // 析构函数
      void print() {          // 对象方法
          cout << cnt << endl;
      }
  };
  int A::cnt = 0; // 类属性初始化
  int main() {
      A a;                // 栈上对象
      A *b = new A(1, 1); // 堆上对象
      a.print();          // 调用方法
      b->print();
      delete b; // 堆上对象需要手动删除
      b = nullptr;
  }
  ```
- 什么是`this`指针？
  - 每个类成员方法都包含一个名为`this` 的隐藏指针（包括构造和析构函数）
  - 指针存放当前对象的地址
- `this`指针有什么用处？
  - 用来解决实参和形参**重名**的问题
  - 打印对象地址
  - 链式调用（包括返回指针和返回引用）
  ```cpp
  class A {
  public:
      int x; // 对象属性
      A(int x) : x(x){};
      // 指针形式
      A *set_point(int x) {
          this->x = x;
          return this;
      }
      A *print_point() {
          cout << x << endl;
          return this;
      }
      // 引用形式
      A &set_ref(int x) {
          this->x = x;
          return *this;
      }
      A &print_ref() {
          cout << x << endl;
          return *this;
      }
  };
  int main() {
      A a(1);          // 栈上对象
      A *b = new A(2); // 堆上对象
      a.set_ref(3).print_ref(); // 引用
      b->set_point(2)->print_point(); // 指针
      delete b; // 堆上对象需要手动删除
      b = nullptr;
  }
  ```
- 在C++中Getter和Setter如何实现？
  ```cpp
  class A {
  private:
      int x; // 对象属性
  public:
      A(int x) : x(x){};
      // Getter
      int get_x(){ return this->x; }
      // Setter
      int set_x(int x){ this->x = x; }
      A *set_point(int x) {
          this->set_x(x);
          return this;
      }
  };
  ```
  可以避免直接操作类对象
### 3 const、不可变对象
- 如何调用`const` 对象的方法？
  - `const` 对象只能调用 `const` 方法，可变对象也可以调用 `const` 方法
  - `const` 方法 就是在函数名后加 `const` 修饰。加了 `const`, 就不能在函数内修改对象属性了
  ```cpp
  int get_x() const { return this->x; }
  ```
  - `const` 对象需要对应的指针和引用也要加 `const`
- 函数形参设为`const`会发生什么？
  - 函数内这个参数就不可变了
  - 调用函数是传可变对象进来可以正常运行
- 不可变对象是否可以传入非`const`函数？
  - 不可变对象值可以，引用、指针不可以
  ```cpp
  void func (A a){ cout << "test" ; }
  void func_ref (A& a){ cout << "test" ; }
  void func_point (A* a){ cout << "test" ; }
  int main() {
      const A a(1);
      func(a);        // 可以
      const A& b{a};
      func_ref(b);    // 不可以
      const A* c{&a};
      func_point(c);  // 不可以
  }
  ```
- 什么是可变引用和不可变引用的动态返回？
  - 我们希望调用一个`const`对象的`getter`能返回`const`的成员变量，反之返回可变的成员变量
  ```cpp
  // 类 A 中定义如下两个方法
  int &get_x(){ return this->x; }
  const int &get_x() const { return this->x; }
  ...
  A a(1);
  a.get_x() = 2;
  const A b(1);
  b.get_x() = 2; // 不能运行
  ```
- `mutable` 关键字如何使用？有什么应用场景？
  - 使用 `mutable`修饰的成员变量，可以在 `const` 方法中被修改
  - 一般用在统计调用次数
  ```cpp
  class A {
  private:
      mutable size_t cnt;    
  public:
      A(int x) : cnt(x){};
      void print() const {
          this->cnt++;
          cout << "print" << ' ' << this->cnt << endl;
      }
  };
  int main() {
      const A a(0);
      A b(0);
      a.print(), a.print(), a.print();
      b.print(), b.print();
  }
  ```
### 4 构造函数
- 构造函数和析构函数在什么时候会被调用？
  - 在**创建**和**销毁**实例时候被调用
  - 如果对象定义在栈上，作用域结束会自动销毁
  - 调用 `delete` 时也会销毁
- 构造函数有什么特点？
  - 没有返回值
  - 函数名为类名
  - 参数可以多种多样，可以为空
  - 一般用于初始化实例属性
  ```cpp
  class A {
  private:
      int x, y, *z;
  public:
      A(int x, int y, int z) : x(x), y(y), z(new int{z}){
          cout << "Three" << endl;
      }
      A(int x, int y) : A(x, y, 0){
          cout << "Two" << endl;
      }
      A(int x) : A(x, 0){
          cout << "One" << endl;
      }
      ~A(){
          delete z;
          cout << "delete" << endl;
      }
  };
  int main() {
      const A a(0);
  }
  // 执行结果为：
  // Three
  // Two
  // One
  // delete
  ```
- `default` 构造函数需要注意什么？
  - 如果定义了构造函数，则`default`不会启用
  - `default`构造函数对指针对象会设置成空指针，容易出现错误
  ```cpp
  class A {
  private:
      int *x;
  public:
      A() = default; // 手动启用 default 构造函数
      A(int x) : x(new int{x}){}
      void print_obj() const {
          cout << *x << endl; // 会报错 EXC_BAD_ACCESS
      }
  };
  int main() {
      A a;
      a.print_obj();
  }
  ```
- 如何关闭隐式转换？
  - 在构造函数前加 `explicit`
  ```cpp
  class A {
  public:
      string s;
      // A(string s) :s(s){}
      explicit A(string s) :s(s){}
  };
  bool compare(A a, A b) {return a.s < b.s;};
  int main() {
      A a("a");
      string s{"b"};
      cout << compare(a, s) << endl;
  }
  ```
  > 不使用`explicit`的话，`s`会自动从`string`类型隐式转换成`A`类型。
- 什么是拷贝构造函数？深拷贝和浅拷贝的区别？
  - 使用相同类型的对象的构造函数
  - 深拷贝：对于成员变量中的指针，会new一个新的对象赋相同的值
  - 浅拷贝：对于成员变量中的指针，会指向同一个对象
  ```cpp
  class A {
  public:
      string *s;
      A(string s) :s(new string{s}){}
      // 错误copy构造函数会循环调用
      // A(const A a){}
      // 浅拷贝
      A(const A &a) :s(a.s){}
      // 深拷贝
      A(const A &a) :s(new string(*a.s)){}
  };
  ```
- 什么是 move 构造函数？
  - 从临时值（包括右值）获取数据
  - 使用std::move时，会调用 move 构造函数
  ```cpp
  class A {
  public:
      string *s;
      A(string s) :s(new string{s}){}
      string* move_s(){
          string *new_s{s};
          s = nullptr; // move 之后就没了
          return new_s;
      }
      // move 构造函数
      A(A &&a) :s(a.move_s()){}
  };
  int main() {
      A a(move(A("xxx")));
  }
  ```
### 5 struct、class
- `struct` 和 `class`的区别？
  - 主要区别是 `struct`默认 `public` , `class` 默认 `private`
