# Preprocessor & Inline Functions

#include " " or < > ?

前者：先在项目配置的头文件引用目录中查找，再在系统类库目录里查找

后者：仅在系统类库目录里查找



## Inline Functions(C++)

### Concept & Features

- Multiple copies of the function code are inserted in the program.

- Each occurrence of a call of the function should be replaced with the code that implements the function.

- **Small** & **Frequently**-used Functions

### Advantages

- Reduce function call overhead and execution time.
- More efficiently.

### Disadvantages

- Make the program(executable image) larger.


### Differences from Macro

- Expansion: macro is expanded by the **preprocessor** and inline function is expanded by the **complier**.
- Semantics: macro regard and inline functions take into account.
- Type-safety Checking: macro doesn't and inline functions does.
