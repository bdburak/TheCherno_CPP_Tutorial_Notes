When we use the `new` keyword, you create whatever data type or object on the heap. 

```cpp
int main(){
	//stack allocated a
	int a = 2;

	//heap allocated b array
	int* b = new int[50]; //200 bytes

	//entity object array allocated in heap
	entity* e = new entity[50];
}
```

The new keyword basically calls the [malloc](https://www.tutorialspoint.com/c_standard_library/c_function_malloc.htm) function that takes in a size parameter and returns a null pointer to that address.

This would be the equivalent of this:

```cpp
entity* e = new entity();
entity* e = (entity*)malloc(sizeof(entity));
```

The only difference between these lines of code is, the second one doesn't call the constructor of that class.

>Normally you should not allocate memory in C++ like the second line. It was just to demonstrate how new worked.

>When you use the `new` keyword, you must remember to use `delete` to delete the variables you don't need.


## Specifying where The Memory Address Should Be
You can also specify where `new` will allocate your object. You can do that using `new(pointer)`:

```cpp
entity* e = (entity*)malloc(sizeof(entity));

entity* e1 = new(e) entity();

```