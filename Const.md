Const, by itself, doesn't do much. We use it as sort of a promise to not change things. You technically can break these promises if you really wanted.

The most simple way of implementing a const is like this:

```cpp
const int a = 5;

//this will give an error
a = 2;
```


## Const Pointers
If you declare a pointer as const, you will not be able to change the content it points to. An example would be; you can change the memory address that const pointer points to, but you can't change the actual value at that address. It locks down that address if you try to modify it through that pointer variable.

```cpp
const int MAX_AGE = 90;

//allocated a in heap
const int* a = new int:

//This would give us an error because we are trying to change the content in the address a is pointing to
*a = 2;

//changing the memory address of a to point to MAX_AGE's memory address. This would work.
a = (int*) &MAX_AGE;

//This would be fine because it's just reading the variable
std::cout << *a << std::endl;

```

You can use it like this aswell:

```cpp
const int* a = new int:
int const* a = new int:
```

The key here is that const is before the variable name.

If you don't declare the pointer a const, but declare the variable as a const. You will get the exact opposite effect. Where you can change the value at that address, but you can't change the address it points to.

```cpp
const int MAX_AGE = 90;

//allocated a in heap
int* const a = new int:

//This would work fine in this case
*a = 2;

//changing the memory address it points to wouldn't work.
a = (int*) &MAX_AGE;

//This would be fine because it's just reading the variable
std::cout << *a << std::endl;

```

You can also just make everything about your variable read-only. 

```cpp
const int MAX_AGE = 90;

//allocated a in heap
const int* const a = new int:

//This wouldn't work because we are changing the content of the address
*a = 2;

//changing the memory address it points to wouldn't work.
a = (int*) &MAX_AGE;

//This would be fine because it's just reading the variable
std::cout << *a << std::endl;
```

## Classes And Methods

You can use const in a class method. When you don't want the class method to modify anything about a variable inside that class, you can do this:

```cpp
class entity {
private:
	int m_x, m_y;
public:
	int getX() const {
		//You can't do this it would give an error
		m_x = 2;
	
		return m_x;
	}

	//you wouldn't want this to be a read-only method
	int setX(int x) {
		m_x = x;
	}
};
```

It will make that method a read-only method.

You can also get crazy and do this:

```cpp
class entity {
private:
	//Made them into pointers
	int* m_x, m_y;
public:
	const int* const getX() const {
		return m_x;
	}
};
```

What we have done is :
- We are returning a pointer that cannot be modified.
- The contents of that pointer cannot be modified.
- And the `getX()` method promises not to modify the actual class variables, such as `m_x` and `m_y`.

> By the way, if you want every variable in the same line to be a pointer, you have to put a `*` before each one.
```cpp
int* a, *b, *c...;
```

#### Why Would We Declare Functions As Const
You might question yourself if even declaring a function as const in a class even enforces something. But it does. When you have a const type function in a class. It guarantees that the variables returned from that function wont be modified. We can use it like this:

```cpp
class entity {
private:
	int m_x, m_y;
public:
	int getX() const {
		//You can't do this it would give an error
		m_x = 2;
	
		return m_x;
	}
	int getY() {
		//You can do this if you wanted
		//m_y = 2;
	
		return m_y;
	}

	//you wouldn't want this to be a read-only method
	int setX(int x) {
		m_x = x;
	}
};

//this wouldn't have to copy the entity
void printEntityX(const entity& e){
	std::cout<< e.getX() << std::endl;
}

//this would have to copy it
void printEntityY(entity& e){
	std::cout<< e.getX() << std::endl;
}
```

When we pass an object as a reference as a parameter, normally it gets copied. But if we can guarantee that the variable wont be modified i.e. a const. We can pass it as a const variable reference and it wont be copied.

You can create const functions to use with const parameter accepting functions outside of a class. Sometimes we create the same function with and without the const, so we can use it in different scenarios.

```cpp
class entity {
private:
	int m_x, m_y;
public:
	int getX() const {
		//You can't do this it would give an error
		//m_x = 2;
	
		return m_x;
	}
	int getX() {
		//You can do this
		//m_x = 2;
	
		return m_x;
	}
	int getY() {
		//You can do this if you wanted
		//m_y = 2;
	
		return m_y;
	}

	//you wouldn't want this to be a read-only method
	int setX(int x) {
		m_x = x;
	}
};

//this wouldn't have to copy the entity
void printEntityX(const entity& e){
	std::cout<< e.getX() << std::endl;
}

```

In the example above, the `printEntityX()` function, only accepting const parameters, would use the const type `getX()` function using overloading.

If you really needed to modify a class variable inside a const function, you can use the [mutable](Mutable.md) keyword before the variable declaration:

```cpp
class entity {
private:
	int m_x, m_y;
	mutable int var;
public:
	int getX() const {
		//you can do this
		var = 2;
		
		return m_x;
	}	
};
```
