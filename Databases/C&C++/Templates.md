# Templates

- **Generic programming**：programming independent of any particular data types by which data(objects) of different types can be manipulated by the same codes.
- Particular data type is determined when the instance code is created from generic code.
- **Types** of Templates: function template and class template



## Function Templates

```c++
template  < class/typename 模板形参表 >
返回值类型 函数名(形式参数列表)
{...}
```

- 函数调用的静态绑定规则
  1. 如果某一**同名非模板函数**的形参类型正好与函数调用的实参类型匹配（完全一致），则调用该函数。否则，进入第2步。
  2. 如果能从**同名的函数模板**实例化一个函数实例，而该函数实例的形参类型正好与函数调用的实参类型匹配（完全一致），则调用该函数实例。否则，进入第3步。
  3. 对函数调用的实参作**隐式类型转换**后与非模板函数再次进行匹配，若能找到匹配的函数则调用该函数。否则，进入第4步。
  4. 提示编译错误。



## Class Templates

```c++
template < class/typename 模板形参表 >
class 类模板名
{...};
```

```c++
template < class/typename 模板形参表 >
返回值类型 类模板名<模板形参名列表>::函数名(函数形参表)
```

- **Inclusion Compilation Model: **在头文件中用#include包含实现文件

```C++
//genericStack.h
#ifndef GSTACK_H
#define GSTACK_H
类模板声明
#include "genericStack.cpp"
#endif 
```

- **Separate Compilation Model: **程序员在实现文件中使用保留字export告诉编译器，需要记住哪些模板定义



## Nontype Parameters

- 非类型模板形参相当于模板内部的常量，形式上类似于普通的函数形参。
- 对模板进行实例化时，非类型形参由相应模板实参的值代替，对应的模板实参必须是常量表达式。
- 可以设置默认参数。
- legal for non-type template parameters: integral, enumeration, pointer or reference to object, pointer or reference to function, pointer to member
- 函数模板中的非类型模板形参

```c++
template <typename T,  std::size_t N>
void printValues(T (&arr)[N])
{
    for (std::size_t i =0; i != N; ++i)
	std::cout<< arr[i] << std::endl;
}

int main()
{
    int intArr[6] = {1, 2, 3, 4, 5, 6};
    double dblArr[4] = {1.2, 2.3, 3.4, 4.5};
	
    printValues(intArr);	    //printValues(int (&) [6])
    printValues(dblArr);	    //printValues(double (&) [4])
}
```