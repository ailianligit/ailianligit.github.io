# Caution

## 考试

- 建一个test.cpp文件
- VSCode配置：C/C++ Code Runner launch.json: "externalConsole" ture
- 首先把所有题打开
- 万能头文件：#include<bits/stdc++.h>

## 细节

- 头文件**被多个文件include**要加preprocessor wrapper，防止重复编译
- sizeof(char*) = 4
- **动态分配字符串**空间时要预留一位给'\0'
- **删除数组**空间要记得加'[]'
- 有需要**加长空间**的情况要记得重新分配空间，要么就一次性分配大空间
- 函数块内**动态分配**的空间要在函数结束前删除
- 使用memset、memcpy等要注意

```c++
memset(a, 0, N * sizeof(int));     		//sizeof(int)!!!
```

- 默认参数**仅出现一次**（单独声明则定义不需要）

```c++
class COMPLEX {
    COMPLEX(double r = 1.0, double i = 2.0);
}
COMPLEX::COMPLEX(double r, double i) {...}   	//no default value
```

- 静态成员变量要记得**定义**
- 只有**构造函数**能用初始化列表
- **除了默认构造函数，定义普通构造函数要记得对所有数据作初始化！！！**
- 类内所有声明的函数都需要**显式定义**
- 友元函数定义时**不需要friend**

```c++
std::istream & operator>>(std::istream &is, fraction &fra) {...}
```

- 抽象类的**析构**函数要记得加virtual
- 重载运算符时若有**+=**，则+也要重载
- 构造与析构顺序

```c++
void Test() {
    Object b(2);
    Object c(3);
}

void TestObjects() {
    Object a(1);
    Test();
    Object d(4);
}

//Object 1 is created, we’ve got 1 object(s) now!
//Object 2 is created, we’ve got 2 object(s) now!
//Object 3 is created, we’ve got 3 object(s) now!
//Object 3 is deleted, we’ve got 2 object(s) now!
//Object 2 is deleted, we’ve got 1 object(s) now!
//Object 4 is created, we’ve got 2 object(s) now!
//Object 4 is deleted, we’ve got 1 object(s) now!
//Object 1 is deleted, we’ve got 0 object(s) now!
```

## 技巧

- 不能用atoi -> **sprintf & scanf**
- 向set中插入**多个**元素

```c++
dset.insert({1.2, 3.4, 3.4});
```

- 如何**逃过析构**

```c++
newLi1.head = newLi1.tail = newLi2.head = newLi2.tail = NULL;
```

- 集合内删除元素

```c++
for(set<double>::iterator it = dset.begin(); it != dset.end(); it++) {
	if(*it >= 3.4 && *it < 7.8) {
		dset.erase(it--);
	}
}
```

- 指针直接交换

```c++
int* tmp = a;
a = seq2.a;
seq2.a = tmp;
```
