# Inheritance

## Inheritance Concept

- **Reusing:** code sharing and interface sharing
- Only **public** form of inheritance is  is-a relationships.
- **Override:** 实现和重定义虚函数
- **Overload:** 重载有相同的函数名和不同的参数列表
- **Redefinition:** 重定义有相同的函数名和参数列表



## Base Classes & Derived Classes

```c++
class 派生类名: 继承访问控制 基类类名{
成员访问控制:
    成员声明列表
};
```

- 继承访问控制和成员访问控制均由保留public、protected、private来定义，缺省**均为**private。

![image-20210703192733518](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078906.png)

- 派生类的对象不仅存放了在派生类中定义的非静态数据成员，而且也存放了从基类中继承下来的非静态数据成员。

- 调用对象的成员函数：先在派生类中寻找，然后到基类中寻找。若两个基类中有相同名字的成员，则使用**域运算符**或在派生类中**重定义**。

- 赋值运算的类型兼容性：可以将后代类的对象赋值给祖先类对象，反之不可。指向基类对象的指针也可指向公有派生类对象，只有**公有派生类**才能兼容基类类型。

- **Resuming** the access control:

```c++
public:
	基类名::成员名;	//放于适当的成员访问控制后
```



## Multiple Inheritance

- **Duplicate inheritance: **There are several copies of the base class members in a derived class object.
- 菱形继承中的基类成员访问（**Ambiguity** in duplicate inheritance）：

```c++
class BASE {
public:
     int i;
};
class BASE1: public BASE {};
class BASE2: public BASE {};
class DERIVED: public BASE1, public BASE2 {};

DERIVED set(int a, int b) {
    DERIVED newOne;
    newOne.BASE1::i = a;            //without BASE!!!
    newOne.BASE2::i = b;            //class "::" object "."
    return newOne;
}
```

- **Shared inheritance: **Using **virtual** base to guarantee only one base class copy in derived class object.



## Constructors & Destructors in Derived Classes

- 构造与析构顺序：先虚基类构造函数，后基类构造函数（**声明**顺序），后对象成员构造函数（**声明**顺序），最后是本类的构造函数，析构正好相反。

```c++
派生类名(形参表): 基类名(实参表) {
	...
};
```

```c++
ExtTime::ExtTime (int initHrs, int initMins, int initSecs, ZoneType initZone): Time(initHrs, initMins, initSecs) {
    zone = initZone;
}

void ExtTime::Set (int hours, int minutes, int seconds, ZoneType timeZone) {
    Time::Set(hours, minutes, seconds);
    zone = timeZone;
}
```

- **Base-class constructors, destructors and overloaded assignment operators** are not inherited by derived classes.
- Derived-class constructors, destructors and overloaded assignment operators can call base-class versions.
- A derived class’s constructor **can explicitly invokes** its base class’s constructor. A base class’s constructor **cannot invokes** its derived class’s constructor.
- A derived class’s destructor **cannot invoke** its base & derived class’s destructor.

### Inheriting Base Class Constructors

- Inherit a **base** class’s constructors:

```c++
using BaseClass::BaseClass;
```

- The generated constructors perform only **default initialization** for the derived class’s additional data members.
- By default, each inherited constructor has the **same access level** as its corresponding base-class constructor.
- The default, **copy and move** constructors are not inherited.
- If a constructor is deleted in the base class by placing = delete in its prototype, the corresponding constructor in the derived class is also **deleted**.
- If the derived class does not explicitly define constructors, the compiler generates a **default constructor** in the derived class—even if it inherits other constructors from its base class.
- If a constructor that you explicitly define in a derived class has the **same parameter list** as a base-class constructor, then the base-class constructor is not inherited.
- A base-class constructor’s default arguments are not inherited. Instead, the compiler generates overloaded constructors in the derived class.

```c++
BaseClass( int = 0, double = 0.0 );
DerivedClass( int );
DerivedClass( int, double );
```
