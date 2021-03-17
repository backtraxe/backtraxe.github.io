# C++ 教程


<!--more-->

## A

**常量指针：**

常量指针本质上是个指针，只不过这个指针指向的对象是常量。

特点：

- const 的位置在指针声明运算符 * 的左侧。只要 const 位于 * 的左侧，无论它在类型名的左边或右边，都表示指向常量的指针。
- （可以这样理解，* 左侧表示指针指向的对象，该对象为常量，那么该指针为常量指针。）

```cpp
const int * p1;
int const * p2;

const int c_var = 8;
int var = 6;
const int *p3 = &c_var;
*p3 = 6;       // error C3892: “p3”: 不能给常量赋值
p3 = &var;  // 可以赋值
```

> 指针指向的对象不能通过这个指针来修改，也就是说常量指针可以被赋值为变量的地址，之所以叫做常量指针，是限制了通过这个指针修改变量的值。
> 虽然常量指针指向的对象不能变化，可是因为常量指针本身是一个变量，因此，可以被重新赋值。

**指针常量：**

指针常量的本质上是个常量，只不过这个常量的值是一个指针。

特点：

- const 位于指针声明操作符右侧，表明该对象本身是一个常量，* 左侧表示该指针指向的类型，即以 * 为分界线，其左侧表示指针指向的类型，右侧表示指针本身的性质。

```cpp
const int c_var = 8;
int var = 6;
int * const c_p1 = &c_var;  // error C2440: “初始化”: 无法从“const int *”转换为“int *”
int * const c_p2 = &var;
c_p1 = &var;  // error C3892: “c_p1”: 不能给常量赋值
*c_p2 = 6;
```

> 指针常量的值是指针，这个值因为是常量，所以指针本身不能改变。
> 指针的内容可以改变。

## B

重载：

是指同一可访问区内被声明几个具有不同参数列（参数的类型、个数、顺序）的同名函数，根据参数列表确定调用哪个函数，重载不关心函数返回类型。

```cpp
class A {
public:
    void fun(int tmp);
    void fun(float tmp);            // 重载 参数类型不同（相对于上一个函数）
    void fun(int tmp, float tmp1);  // 重载 参数个数不同（相对于上一个函数）
    void fun(float tmp, int tmp1);  // 重载 参数顺序不同（相对于上一个函数）
    int fun(int tmp);               // error C2556: “int A::fun(int)”: 重载函数与“void A::fun(int)”只是在返回类型上不同
};
```

隐藏：

是指派生类的函数屏蔽了与其同名的基类函数，主要只要同名函数，不管参数列表是否相同，基类函数都会被隐藏。

```cpp
class Base {
public:
    void fun(int tmp, float tmp1) { cout << "Base::fun(int tmp, float tmp1)" << endl; }
};

class Derive : public Base {
public:
    void fun(int tmp) { cout << "Derive::fun(int tmp)" << endl; }  // 隐藏基类中的同名函数
};

int main() {
    Derive ex;
    ex.fun(1);       // Derive::fun(int tmp)
    ex.fun(1, 0.01); // error C2660: “Derive::fun”: 函数不接受 2 个参数
    return 0;
}
```

重写(覆盖)：

是指派生类中存在重新定义的函数。函数名、参数列表、返回值类型都必须同基类中被重写的函数一致，只有函数体不同。派生类调用时会调用派生类的重写函数，不会调用被重写函数。重写的基类中被重写的函数必须有`virtual`修饰。

```cpp
class Base {
public:
    virtual void fun(int tmp) { cout << "Base::fun(int tmp)" << endl; }
};

class Derive : public Base {
public:
    virtual void fun(int tmp) { cout << "Derive::fun(int tmp)" << endl; }  // 重写基类中的同名函数
};

int main() {
    Base *p = new Derive();
    p->fun(3);  // Derive::fun(int)
    return 0;
}
```

