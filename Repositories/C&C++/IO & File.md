# IO & File

## Manipulators in C++

### iostream

- ##### fixed & showpoint


```c++
#include <iostream>
cout << fixed << showpoint;
```

### iomanip

- **setprecision(n)**
  - 无fixed配合时，对**浮点数**设置总的有效位数；有fixed配合时，设置小数点后的位数
  - setprecision设置**长期有效**

```C++
#include <iomanip>
cout << setprecision(10) << 1.233666 << endl << 99999 << endl;
/* 
without fixed & showpoint
1.233666
99999
with only fixed
1.2336660000
99999
with only showpoint
1.233666000
99999
with both fixed & showpoint
1.2336660000
99999
*/
```

- **setw(n):** 默认为右对齐，setw设置仅对**紧跟其后的一个**输出项有效。

- **setfill('*'):** 配合setw函数使用，**永久有效**，直至遇到下一个setfill函数。



## Text Files & Binary Files

- 文本文件输出，需要先将输出值转化为字符序列，然后存储到文件中。文本文件输入，需要先将字符序列转化为相应的数值，然后再进入变量中。

- 二进制文件输出，是直接将内存数值（即其二进制形式）输出到文件中保存。因此二进制文件的内容与内存的形式一致。二进制文件输入，直接将文件内容输入到内存中即可，无需转化过程。

  

## File Processing in C

### FILE & fopen

```c
FILE *fp;
fp = fopen( 文件名, 文件使用方式 ); 
```

![image-20210706203141742](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210706203141742.png)

- 打开动作并非一定成功，打开失败时fopen返回NULL。
- 若要写的文件不存在，则将**先创建**这个文件，然后再往这个文件里面写入数据。
- “w”：若要写的文件已存在，则先**清空**文件的原本内容，然后再写入数据。
- “a”：若要写的文件已存在，不清除文件的原本内容，从文件尾**续加**新数据。

### fread & fwrite

```c
fread( buffer, size, count, fp1 );
fwrite( buffer, size, count, fp2 );
```

![image-20210706203423091](C:\Users\Elian Li\AppData\Roaming\Typora\typora-user-images\image-20210706203423091.png)

- fwrite返回实际写入（输出）的数据项的个数，而fread返回实际读入（输入）的数据项的个数。



### fseek

```c
fseek( 文件指针，位移量(字节数)，起始点 );
```

- **SEEK_SET：**文件开始；**SEEK_CUR：**当前位置；**SEEK_END：**文件末尾



## File Processing in C++

```c++
#include <fstream>

ifstream myInfile;
ofstream myOutfile;

myInfile.open("...");
myOutfile.open("...");

myInfile.close();
myOutfile.close();
```



