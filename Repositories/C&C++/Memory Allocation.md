# Memory Allocation

## Dynamic Memory Management

- **unique_ptr:** a “smart pointer” for managing dynamically allocated memory. When a unique_ptr goes out of scope, its destructor automatically returns the managed memory to the free store.



## unique_ptr

- Class template unique_ptr in header "memory" to deal with **memory leak**.
- An object of class unique_ptr maintains a **pointer** to dynamically allocated memory.
- When a unique_ptr object destructor is called (for example, when a unique_ptr object goes out of scope), it performs a delete operation on **its pointer data member**.
- Class template unique_ptr provides overloaded operators * and -> so that a unique_ptr object can
  be used just as a **regular pointer variable** is.



new strcat delete