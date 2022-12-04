# Classes & Objects

## Concept

- **Object-Oriented Programming (OOP):** Encapsulation, Inheritance and Polymorphism
- **Encapsulation:** OOD encapsulates (i.e., wraps) attributes and operations (behaviors) into objects.
- **Information Hiding:** objects may know how to communicate with one another across well-defined interfaces, but normally they’re not allowed to know how other objects are implemented—implementation details are hidden within the objects themselves.
- **Data Members:** attributes
- **Member Functions:** behaviors, operations, methods
- **Object:** instances
- **Client:** other classes or functions that use the class
- **Handles:** an object name, a reference to an object or a pointer to an object.
- **Class Scope:** a class’s data members and member functions (include static data members).
- **Block Scope:** variables declared in a member function. The class-scope variable can be accessed by preceding the variable name with the class name followed by the scope resolution operator (::).
- **Access Functions:** read or display data
- **Predicate Functions:** a kind of access functions. Test the truth or falsity of conditions.
- **Utility Functions:** private member functions that support operation of the class's other member functions.
- Every object of the same class gets a copy of every data members.



## Implementing Details

- An object name is a **constant**,  which cannot be reassigned with a new object.
- An object is associated with **only one** object name (reference exception).
- **"private"** is default type.
- For class, "::"members; for object, "." or "->"members.
- Data members cannot be initialized when declaration.
- **Composition (Containership):** object members

```c++
Time(..., int hrs): Hour(hrs){...}		//Hour is a object member
```

- A declared function in class must be defined **explicitly**.
- Returning a reference or a pointer to a private data member **breaks the encapsulation** of the class and makes the client code dependent on the representation of the class’s data.



## The this Pointer

- The this pointer is **not part** of the object itself.
- The this pointer is passed (by the compiler) as an **implicit** argument to each of the object’s **non-static** member functions.
- In a non-const member function of class Employee, the this pointer has the type Employee *. In a const member function, the this pointer has the type **const** Employee *.



## Constant Members

- Constant object **can only invoke** constant member functions.
- Constant member functions **cannot modify** member variables.
- Defining as const a member function that calls a non-const member function of the class on the same object is a compilation error.
- Attempting to declare a **constructor** or **destructor** const is a compilation error.
- Invoking a non-const member function from the constructor call as part of the initialization of a const object is allowed.
- "const" appears in **both** prototype and definition of functions.
- Constant data members' initialization(non-constant is also valid):

```c++
Time(..., int zone): timeZone(zone){...}		//timezone is a constant data member
```



## Static Members

- Static members are components of **class** but not object.
- A class’s static data members have **class scope**.
- Implementing of static member variables:

```c++
class Time {
private:
	static int count;			//declaration
};
int Time::count = 0;			//definition (without "static" but "Time::")
int main() {
    Time t;
    //cout << Time::count;		//error, valid if public
    //t.count++;				//error, valid if public
}
```

- If a static data member is an **object** of a class that provides a default constructor, the static data member need not be initialized because its **default constructor** will be called.
- **Static Member Function:** access a **private or protected** static class member when no objects of the class exist, provide a **public** static member function and call the function by prefixing its name with the class name and scope resolution operator.
- Static member functions **can only access** static data members.
- A static member function does not have a **this** pointer.
- Declaring a static member function **const** is a compilation error.



## Constructors

```c++
Time newTime;							//construtor without argument 
Time newTime(hrs, min, sec);			//constructor with arguments
Time newTime = Time(hrs, min, sec);		//constructor with arguments
Time newTime = hrs;						//constructor with arguments (implicit)

Time otherTime(newTime);				//copy constructor
Time otherTime = Time(newTime);			//copy constructor
Time otherTime = newTime;				//copy constructor (implicit)

Time* newTime = new Time(hrs, min, sec);
delete newTime;		//invoke the destructor
newTime = NULL;		//for judgement below
```

- Purpose: initialize the private data members of a class object.
- 构造函数能够调用成员函数和其他构造函数（**Delegating Constructor**）.

```c++
Time::Time(): Time(0, 0, 0) {}
```

- 返回值是对象或对象的引用在表达式中作为右值时无明显区别，但作为左值时，返回对象的函数实际上返回的是临时对象，因其在表达式结束后需要被析构，因此作为左值的临时对象修改无意义。

```c++
(a=b)=c;		//left value
a=b=c;			//right value
```

### Default Constructors

- A constructor that defaults all its arguments is also a default constructor—that is, a constructor that can be invoked with no arguments.
- There can be at most one default constructor per class.

### Copy Constructors

- Copy constructor: avoid shallow copy (pointer alias & garbage) and realize **deep copy** when **pass-by-value** and **return object**.
- Default copy constructor use **shallow copy**.
- Copy constructor pass **constant** and **reference** (avoid indefinite invoking of copy constructor) of another object of the class.

- 返回对象时，程序调用拷贝构造函数产生临时对象。若没有显式的拷贝构造函数，程序调用使用浅复制策略的缺省拷贝构造函数，临时对象中的指针与函数中局部对象的指针指向同一块内存空间。函数调用完成，函数中的局部对象被析构，临时对象中的指针也同时被销毁。

### Explicit Constructors

**Implicit Conversion:** With the exception of copy constructors, any constructor that can be called with a single argument and is not declared explicit can be used by the compiler to perform an implicit conversion.



## Destructor

- Invoking explicitly do not release the space of object.
- Objects are implicitly destructed before the end, and the space of these objects will be cleared.



## Others

```c++
class A				//sizeof(A) = 16
{   
private:
    int a;			//sizeof(a) = 4
    char b;			//sizeof(b) = 1 (1->4?)
    double c;		//sizeof(c) = 8
};
```

```c++
class A				//sizeof(A) = 16
{   
private:
    char b;			//sizeof(b) = 1 (1->8?)
    double c;		//sizeof(c) = 8
};
```
