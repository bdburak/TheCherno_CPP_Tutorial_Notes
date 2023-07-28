Variables are stored in the memory. They can be either stored in the stack or heap. 

## `sizeof()`
You can use the `sizeof()` function to see how many bytes a variable takes up.

```cpp
char var = "a";
std::cout << sizeof(var) << std::endl;
std::cout << sizeof(int) << std::endl;
```
This will result in the following console output:

```bash
1
4
```

## [[Pointers]]

Pointers refer us to an address. You can define a variable as  a pointer in the following way:

```cpp
int* var;
```

## [[References]]
References gets us the value of an address. You can define a variable as  a pointer in the following way:

```cpp
int& var;
```