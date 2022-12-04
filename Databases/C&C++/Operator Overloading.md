# Operator Overloading

## Concept

- Cannot be overloaded:	**.	.*(pointer to member)	::	?:**
- Cannot change: "arity", precedence & associativity



## Member Function in Class

```c++
COMPLEX operator+(const COMPLEX&);
c1 + c2;
c1.operator+(c2);

COMPLEX operator-();			//prefix(right-associative)
-c1;
c1.operator-();

COMPLEX& operator++();			//prefix(right-associative)
COMPLEX operator++(int);		//postfix(left-associative)

int& operator[](int);			//modifiable
label[2] = 8;
label.operator[](2) = 8;
```

- if **unary** and **left-associative**, add "int" as parameter.
- Related operators, like **+ and +=**, must be overloaded separately. The += operator cannot be overloaded implicitly.
- operator[]: integers, characters, strings or objects of user-defined classes
- When overloading **(), [], ->** or any of the **assignment** operators, the operator overloading function must be declared as a class member.
- 使用赋值符号时，等号右端的的临时对象被赋值给左值。若没有显式的赋值重载函数，程序调用使用浅复制策略的**缺省赋值运算符**，临时对象中的指针与被赋值对象的指针指向同一块内存空间。赋值表达式调用结束，临时对象被析构，被赋值对象中的指针也同时被销毁。
- 对比下列三种不同返回值的赋值重载函数：三种函数都在函数内完成复制操作。第一种无返回值，无法做连续赋值操作，如a=b=c中先做b=c，没有临时对象产生，无法赋值给a；在赋值重载的情况下，第二种和第三种函数的效果在函数返回值作为右值时无区别，如a=b=c，但是在函数返回值作为左值时有区别，如(a=b)=c，在第二种情况下，第二次赋值时c的值被赋给临时对象，无意义。

```c++
void operator=(const STRING&);
STRING operator=(const STRING&);
STRING& operator=(const STRING&);
```

- How to design a operator= overloading function:


```c++
list& list::operator=(const list& other) {
    if(other._data == _data) return *this;      //same object
    this->~list();
    //data member copy
    //deep copy
    return *this;
}
```



## Friend Function

- **Friend Function:** a **non-member** function that has the right to access the public and non-public class members.
- **Standalone functions**, **entire classes** or **member functions** of other classes may be declared to be friends of another class.
- Friend declaration can appear **anywhere** in the class.
- Friendship is **granted**, not taken.
- Friendship is **not symmetric**—if class A is a friend of class B, you cannot infer that class B is a friend of class A.
- Friendship is **not transitive**.

```c++
class VALUE {
public:
	friend void set(VALUE, int);			//free function
    friend NEWVALUE;						//another class
    friend void NEWVALUE::NewSet(VALUE);	//member function of another class
} 
class NEWVALUE {
public:
    void NewSet(VALUE);					//object as a parameter
};
void set(VALUE ..., int ...);			//without "friend" as prefix
newValue.set(otherValue, 0);			//error: "set" not a member function
```

- **First operand** can be not object of the class.
- Cannot be overloaded in friend function:	**=	()	[]	->**

```c++
friend VALUE operator+(VALUE, VALUE);		//type conversion
```

```c++
#include <iostream>
friend ostream& operator<<(ostream&, const VALUE&);
friend istream& operator>>(istream&, VALUE&);
```



## Converting

- **Conversion operator (Cast Operator):** can be used to convert an object of one class to another type.

- Overloaded cast operator function is declared **const** because it does not modify the original object.

```c++
MyClass::operator int() const;
MyClass::operator OtherClass() const;
```

- **Conversion Constructors:** turn objects of other types (including fundamental types) into objects of a particular class (can be called with a single argument) (without explicit).
