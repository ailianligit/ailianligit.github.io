# Exception Handling

## 异常的抛出

```c++
throw ...;			//object of exception
throw;				//重新抛出异常
void numberValidation(int) throw(LogicError1, LogicError2);		//使用了异常的函数声明
int f1()const throw(logic_error);			//const成员函数
```

```c++
throw MyException("invalid argument");
throw 2.5f;
```

- 在函数原型中，异常类型列表为空，表示该函数不抛出任何异常。
- 不带异常说明的函数可以抛出任意类型的异常。
- 基类虚函数的异常列表是派生类中对应虚函数的异常列表的**超集**。
- **Rethrowing** an exception: `throw;`



## 异常的捕获及处理

```c++
try {
    //program-statements
}
catch (...) {		//exception-declaration
    //handler-statements
}
```

```c++
catch(MyException e) {...}
catch(float) {...}
catch(...) {...}		//任意异常类型均可匹配
```

- 如果找不到匹配的处理代码，则自动调用标准库函数terminate，其默认功能是调用abort()终止程序。
- Exception & **Inheritance**: If a catch handler catches a reference to an exception object of a base-class type, it also can catch a reference to all objects of classes publicly **derived from** that base class.  



## Constructors, Deconstructors & Exception Handling

- Before an exception is thrown by a constructor, **destructors** are called for any member objects whose constructors have run to completion as part of the object being constructed.  

- If an object has **member objects**, and if an exception is thrown before the outer object is fully constructed, then destructors will be executed for the member objects that have been constructed prior to the occurrence of the exception.

- If an **array of objects** has been partially constructed when an exception occurs, only the destructors for the **constructed** objects in the array will be called.  



## Standard Library Exception Hierarchy

**exception：**标准程序库异常类的公共基类

**logic_error：**表示可以在程序中被预先检测到的异常，如果小心地编写程序，这类异常能够避免

**runtime_error：**表示难以被预先检测的异常

![image-20210702013256666](https://raw.githubusercontent.com/ailianligit/images/main/images/202308/20230804_1691078896.png)
