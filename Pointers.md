A pointer is an integer number that stores a memory address.
Memory, in a computer, is a linear 1\*memory length array that holds data. Each address points to a single byte of data.
You can write entire C++ programs without ever using pointers, but pointers help you control memory better. 

## Creating Pointers

You don't need to give a data type to pointers. The simplest pointer you can create is a `void` type pointer.

```cpp
//0 is not a valid memory addres and is just a placeholder
void* ptr = 0; 
//null means 0 in cpp
void* ptr = NULL; 
//this was introduced in cpp 11
void* ptr = nullptr; 

```

If we give a type to our pointer, it is presumed that the data at that address can be presumed to be the type we assigned our pointer as. **It doesn't mean anything.** 

## Assigning The Memory Address Of A Variable As The Value Of A Pointer
```cpp
int var = 8;
void* ptr = &var;
```
>The `&` operator before our variable name will return the memory address of that variable.

## Using Type Cast Pointers To Retrieve Or Edit Variables In Memory
As we covered before, a pointer is only an integer that holds a memory address. That's why you don't need to cast it with a type. But if you want to retrieve or edit a variable in that memory address, you can assign the pointer as the same type as the variable and our program.

### Dereferencing
You can mutate the values of a variable using a pointer with dereferencing. Before, we said that giving your pointer a type can help you in this regard. Our compiler will now that, that pointer points to a specific type that for instance for an integer will point to a 4 byte memory address. You can dereference a pointer, which means access to the value of the address it points, using the `*` operator before the pointer variable name.

```cpp
//creating a variable named var that has a value of 8
int var = 8; 
LOG(var);
//assigning the address of that variable to a pointer called ptr that has the same type as our variable
int* ptr = &var;
LOG(ptr);
log(*ptr);
//accessing and editing the value of var using the dereferenced pointer and assigning the value of 10
*ptr = 10;
Log(var);
```

The output:
```bash
8
0000008CF36FF7F4
8
10
```

## Creating A Variable In The Heap

So far we only created variables in the stack portion of our memory. Now we will create a variable in the heap portion of our memory.

```cpp
//this will allocate an 8 byte char array and put it in the heap
char* buffer = new char[8];

//fills a block of memory with data that we specifieds
//it takes in a pointer, a value, size of our data;
memset(buffer, 10, 8);

//you can delete it after you use it
delete[] buffer;
```

## Double Pointers(Pointers To Pointers)

Pointers are just variables, so we can actually create pointers that point to the address of that pointer. You can create a double pointer with the `**` operator.

```cpp
//the value of the buffer pointer would be 0x8bc9aaf1 and it would point to the beginning of the 8 byte value
char* buffer = new char[8];

//The value of the ptr double pointer would be 0x6bc840d0 and it would point to the beginning of the buffer pointer
char** ptr = &buffer;
```

> The memory is stored in reverse order so you have to reverse it to get the value:
> ![[Pasted image 20230717020730.png]]
> it would be 0x6bc840d0 not 0xd040c86b.
> Keep it in mind.
