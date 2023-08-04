We will explore what lifetime means for stack based variables. There are two parts to this; 
- Understanding how things live on the stack.
- How to leverage the stack to make our code do what we want.

## How The Stack Works
The stack is like a stack of books. Everything has to be added and taken from the top of it. Every time we enter a scope in C++, we push a [stack frame](https://www.geeksforgeeks.org/stack-frame-in-computer-organization/) on it. This is like adding a book to the pile. Any variables that you declare inside that scope is like writing stuff in your book. Once that scope ends, it's like taking that book off of your books and throwing it in the trash. Every stack based variable you declared in that book, every object that you created on the stack are gone. This is both a blessing and a curse.

## Examples

### Scopes
A scope can be a function scope, condition scope, loop scope or an empty scope. The scope is basically what is inside `{}` curly braces. 

```cpp
class entity {
public:

	entity() {
		std::cout << "created entity!" << std::endl;
	}
	~entity() {
		std::cout << "destroyed entity!" << std::endl;
	}
};

int main() {
	{
		entity e;
	}
	std::cout << "Hello world" << std::endl;
}
```

The output:

```
created entity!
destroyed entity!
Hello world
```

When we get to the "Hello world" console print, we will also see that the entity was destroyed. This was caused by the end of the scope. 

if we were to change the stack allocated `e` to a heap allocated `e`, this wouldn't happen because we wouldn't have destroyed it.

```cpp
...
int main() {
	{
		entity e = new entity();
	}
	std::cout << "Hello world" << std::endl;
}
```

The output:

```
created entity!
Hello world
```

As we can see, the `e` object wasn't destroyed and the destructor was never called.

#### What Not To Do After We Learned These Things
The following example is what not to do:

```cpp
int* createArray() {
	int array[50];
	return array;
}
int main() {
	int* array = createArray();
}
```

This might seem normal, but it wouldn't work. The `array` array variable would be destroyed after the scope of `createArray()` function. This in turn would return a pointer that pointed to nothing.

If you want to write a function like this, you have 2 options:
- Either allocate the `array` array on the heap.
```cpp
int* createArray() {
	int array = new int[50];
	return array;
}
int main() {
	int* array = createArray();
}
```
- Copy the data to a variable further up the scope. Where you have an array in let's say your main function. You can populate that array by passing it into the `createArray` function with a pointer and filling it in there.

```cpp
void createArray(int* array) {
	array[0] = 1;
}
int main() {
	int* array[50];
	createArray(array);
}
```

## Leveraging The Stack
There are a lot of way in which we can use the stack to our advantage. 
- We can write scoped classes.
- Scoped pointers such as [smart pointers](Smart Pointers.md)
Let's see how we can write scoped class with scoped pointers:

```cpp
...
//basic scoped pointer
class scopedPointer {
private:
	entity* m_ptr;
public:
	scopedPointer(entity* ptr) 
		:m_ptr(ptr)
	{
		
	}
	~scopedPointer()
	{
		delete m_ptr;
	}
};

int main() {
	{
		//this is the same as scopedPointer e = scopedPointer(new entity);
		scopedPointer e = new entity;
	}
	std::cout << "Hello world" << std::endl;
}
```

The output:

```
created entity!
destroyed entity!
Hello world
```

Even though we used the `new` keyword to create an `entity` object, it still got deleted after it got out out scope. This happened because we created our `scopedPointer` object on the stack and when it got out of scope, it called its own destructor, which also deleted the `entity` object. 

Our `scopedPointer` class it the same thing as a [smart pointer](obsidian://open?vault=c%2B%2B&file=Smart%20Pointers). 

You can also write a timer to see how long you are inside a scope. You can write a timer class, which starts the timer when the object is created and stops and logs it when it's destroyed.