# Polymorphism

## Types of Polymorphism

- **Compile-time polymorphism:** association done in compile time
  - function overloading
  - operator overloading

- **Run-time polymorphism:** association done during run time, implemented by **dynamic binding**
  - **inheritance** plus **virtual function**



## Binding

- Binding is the process of associating a function call and a function definition.
- **Static binding (Early binding):** Done during compile-time, applied to non-virtual function.
- **Dynamic binding (late binding):** Done during run-time, applied to **virtual function**.
- When **pass-by-value** is used, **static binding** occurs, causing **slicing**.



## Virtual Function

- **Polymorphism Class:** class that contains virtual function(s).
- 调用普通成员函数采用静态绑定方式。通过指针（或引用）调用普通成员函数，仅仅与指针（或引用）的基类型有关，而与该指针当前所指向（或引用当前所关联）的对象无关。
- 与继承相结合以实现运行时多态性。在公有继承层次中的一个或多个派生类中对虚函数进行重定义，然后通过**指向基类**的指针（或引用）调用虚函数来实现实现运行时多态性。
- 当一个派生类没有重新定义虚函数时，则使用其基类定义的虚函数版本。
- 在派生类中**重定义**从基类中继承过来的虚函数（函数原型保持不变），重定义的函数在派生类中仍是虚函数。
- 函数**重载**，**虚特性**丢失。
- **Virtual Destructor:** virtual destructor must be used when delete is used on a base-class pointer to a derived-class object.



## Pure Virtual Function

- **Abstract Class:** a class that contains **pure** virtual function.
- **Pure Virtual Function:** a virtual function declared but without definition within a base class. Each derived class of this base **must redefine and implement** this function.

```c++
virtual 返回值类型 函数名(形参表) = 0;
```

- Can only be used as a **base** class.
- Can not declare **objects** of an abstract class.
- Can declare **pointers** or **references** of an abstract class. Can not be used for **parameter type** or **returned type** of a function.
- Can not be used in **explicit conversion**.