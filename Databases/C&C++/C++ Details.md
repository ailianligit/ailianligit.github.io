# C++ Details

## Name Space

- cin/cout/cerr/endl/string...

```c++
std::cin >> a;
```

```c++
using std::cin;
```

```c++
using namespace std;
```



## Unary Scope Resolution Operator

```c++
::x = 2;		//assign to global
```

- Always using the unary scope resolution operator (::) to refer to global variables.



## Casts

- **static_cast<...>**
  - 基本数据类型之间的转换
  - 如果对象所属的类重载了**强制类型转换运算符**T（如 T 是 int、int* 或其他类型名），则 static_cast 也能用来进行对象到 T 类型的转换。
  - 类层次结构中基类和派生类之间的指针或引用的转换**（不检查继承关系）**
- **dynamic_cast<...>**
  - 只支持**向上**继承关系转换**（downcast pointer）**
- **const_cast<...>**
  - 常量指针转化为**非常量**的指针，并仍然指向原来的对象（去除const属性）
- **reinterpret_cast<...>**
  - 用于进行各种不同类型的指针之间、不同类型的引用之间以及指针和能容纳指针的整数类型之间的转换。



## References

- 3 ways to pass arguments: pass-by-value, pass-by-reference, (pass by pointer?)
- References must be **initialized**.
- 非常量引用的初始值必须为左值，换言之，常量引用的初始值可以是右值。

```c++
int a = 1;
//int &b = a + 1;			//invalid
const int &b = a + 1;		//valid
const int &c = 10;			//valid
```

```c++
void fun(int &a) { cout << a << endl; }		//without "const"

// 1.binding reference of type 'int&' to 'const int' discards qualifiers
int main() {
    const int a = 2;
    fun(a);
}
// 2.cannot bind non-const lvalue reference of type 'int&' to an rvalue of type 'int'
int main() {
    double a = 2.3;
    fun(a);					//Implicit type conversion (double->const int)
}
int main() {
    fun(2);
}
```
