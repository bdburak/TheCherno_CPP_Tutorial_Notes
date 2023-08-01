Before starting to learn arrays, you need to now what [[Pointers]] are. Arrays are a collection of variables.

## Array Operations

### Defining And Accessing An Array

```cpp
//defining an array of 5 integers
int example[5];

//assigning a value to the 2nd item(1st index)
example[1] = 10;

//printing the 2nd item(1st index)
cout << example[1] << endl;

//this would just print the memory address of the array
cout << example << endl; 
```

#### Memory Access Violation
If you try to assign a value to an out of scope array member, you will get a memory access violation error. This happens when you try to access, or assign, a memory address that doesn't belong to your program.

A simple violation:

```cpp
int example[5];

//A violation
example[10] = 10;
```

>In release mode, this error wouldn't happen and your program could access memory that it shouldn't. So be careful.

### Array For Loop

Accessing an array through a for loop:

```cpp
int example[5];

//instead of this:
example[0] = 0;
example[1] = 1;
example[2] = 2;
example[3] = 3;
example[4] = 4;

//We will do this:
for(int i = 0; i < 5; i++){
	example[i] = i;
}

```


## How Arrays Are Stored In Memory

Arrays are stored continually, side by side, in memory. This means if you create an int array of size 5 and the starting address is 0x0, your first item would start from 0x0 and end in 0x3. The second item would start from 0x4 to 0x7...  
![[Pasted image 20230722031546.png]]


### Accessing Arrays Through Pointers

An array variable only holds the memory address, it is a pointer to that memory address, and can be treated as a pointer. 

We can access members of an array through simple [pointer arithmetic](https://www.tutorialspoint.com/cprogramming/c_pointer_arithmetic.htm) operations.

>Accessing the n-th member of an array:
```cpp
int example[3];

for(int i = 0; i < 3; i++){
	example[i] = i*10;
}

//we can do this because the example variable is just a pointer to the beginning of our array.
int* ptr = example;

for(int i = 0; i < 3; i++){
	cout << *(ptr + i) << endl;
}
```

This would output something like this:

```
0
10
20
```

The kind of array and the kind of pointer you choose has to be the same. For instance, both our array variable and our pointer were integer type pointers. 

Normally you would think that you need to add 4 bytes instead of 1 to get to the next memory location. But the pointer doesn't work that way. We declared our pointer to be an integer, so it will point to an integer's address. Which would start from an address and span 4 bytes. So if you increment the pointer by 1, you are essentially adding 4 to the address because integers are 4 bytes in C++.

An example would be:

```cpp
int intVariable = 20;
int* intPointer = &intVariable;
int* incrementedIntPointer = intPointer + 1;

cout << "int address: " << intPointer << endl;
cout << "incremented int address: " << incrementedIntPointer << endl << endl;
```

The Output would be:

```
int address: 000000B5264FF654
incremented int address: 000000B5264FF658
```

### Creating Arrays On The Heap

Similarly to how we can create class instances on the heap using the `new` keyword, we can do the same for arrays: 

```cpp
//Array created on the stack
int stackArray[5];

//Array created on the stack
int* heapArray = new int[5];
```

Both of our arrays here are the same. But the lifetimes will change.

- The array that was created on the stack will be deleted after it gets out of scope, it isn't used anymore. 
- The array that was created on the heap will stay on the heap until it is manually deleted or the program ends. 

#### Deleting Heap Allocated Arrays
We will use the `delete` keyword to delete our heap allocated array:

```cpp
//Array created on the stack
int* heapArray = new int[5];

//... we do stuff...

//Deleting our heap allocated array from the heap
delete[] heapArray;
```

We created our array with the array `[]` operator, so we have to use the same operator to delete it.

#### Why Create Arrays On The Heap?
As with lots of things, it is about lifetime. We will be able to control when a heap allocated array will be deleted. This helps us achieve a higher degree of control.

If you also want to return an array from a function, you have to use the `new` keyword. Unless you pass an array's memory address as a parameter. If you actually Want to return an array, you ***have to*** use the `new` operator to create a brand new array that was created in the function. 

#### Problems With Creating Heap Allocated Arrays
Heap allocated arrays will cause `memory indirection`. This means, when you create heap allocated arrays, in for example classes, the instances of classes will only have the memory location of that array instead of having it by itself. It is difficult to explain, so an example can help:

```cpp
//has a stack allocated array
class A {
public:
	int variable[3];

	A() {
		for (int i = 0; i < 3; i++) {
			variable[i] = i * 2;
		}
	}
};

//has a heap allocated array
class B {
public:
	int* variable = new int[3];

	B() {
		for (int i = 0; i < 3; i++) {
			variable[i] = i * 2;
		}
	}
};
int main(){
	A aInstance;
	B bInstance;
}
```

The memory content for both would be:

>Stack allocated A memory content at A pointer
>![[Pasted image 20230722045057.png]]

>Heap allocated B memory content at B pointer
>![[Pasted image 20230722045138.png]]

As you can see; 
- A class instance, which has a stack allocated array, contains the actual values of the array at the `aInstance` memory location.
- B class instance, which has a heap allocated array, contains the address of the array from  `bInstance`. We need to follow that address to get to the actual array, which adds to the complexity and the indirectness. 

> So whenever possible you should create your variables in the stack. to avoid jumping around memory, which would have a performance loss.  

### Size Of An Array
You can only get the size of a stack allocated array and not a heap allocated array:

```cpp
int a[5];
//       a array size in bytes / int size in bytes
int countA = sizeof(a) / sizeof(int): //5

//the same can't be done for heap allocated arrays
int* b = new array[5];
//this would give you the integer pointer size / int size which would be 1
int countB = sizeof(b) / sizeof(int):
```

## C++ 11 Arrays
In C++ 11 we got something called the `standard array`, which is an actual data structure built into C++ 11 library. It includes lots of advantages compared to the arrays that C++ normally has. Some of which are:

- Includes bounds checking (you can't access array indexes more than the array size).
- Keeps track of the size of our array, compared to just creating a pointer to the beginning of an array.

You can create a `standard array` like so:

```cpp
#include <array>

std::array<TYPE_NAME, SIZE_OF_ARRAY> variableName;
```

An example of creating an array and getting the size of it:

```cpp
#include <iostream>
#include <array>

std::array<int, 5> variable;

for (int i = 0; i < variable.size(); i++) {
	variable[i] = i * 2;
}
```

> Note that `.size()` only works for `standard array`'s and not normal C++ arrays.

There usually is a bit of overhead to using `standard array`'s, but it generally is worth it. 