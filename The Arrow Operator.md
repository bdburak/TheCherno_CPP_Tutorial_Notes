In this tutorial we will see how the arrow operator works for both classes and structs. And we will create our own arrow operator to see how it works under the bonnet.

## Using The Arrow Operator
Here we can see a simple use case of the arrow operator:

```cpp
class entity {
public:
	void print() const { std::cout << "hello!" << std::endl; }
};

int main() {
	entity e;
	e.print();

	entity* ptr = &e;
	//This wouldn't work because ptr is a pointer
	ptr.print();
}
```

As you can see, we can't access the member variables or functions of the `ptr` pointer. Even though it is a pointer to `e`. We have to dereference `ptr` to access the member variables and functions. This would look something like this:

```cpp
...
int main() {
	entity e;
	e.print();

	entity* ptr = &e;
	(*ptr).print();
}
```

This works but it looks really messy. We can remedy that by using the arrow operator `->`. It automatically dereferences our pointer so that we can tidy up our code.

```cpp
...
int main() {
	entity e;
	e.print();

	entity* ptr = &e;
	ptr->print();
}
```

Another use case for the arrow operator is within classes. We can access the instance's members through it using the arrow operator:

```cpp
class entity {
private:
	int x, y;
public:
	entity(int x, int y){
		this->x = x;
		this->y = y;
	}
	void print() const { std::cout << "hello!" << std::endl; }
};
...
```

You can't access them with `.`.  You can also access ***public*** members with a pointer:

```cpp
...
int main() {
	entity e;
	e.x = 10;
	e.print();

	entity* ptr = &e;
	p->x = 10;
	ptr->print();
}
```

As an operator in C++, you can overload the arrow operator and make it do whatever you want.

## Overloading The Arrow Operator
We will go over an example where we can overload the arrow operator of a class to make our life easier:

```cpp
...
class scopedPtr {
private:
	entity* m_obj;
public:
	scopedPtr(entity* entity)
		: m_obj(m_obj) 
	{		
	}
	~scopedPtr() {
		delete m_obj;
	}
}
```

This is our scoped pointer class. Here we can create scoped pointers for entity objects. If we want to access the members of the entity object that we pass in it, we have to either not declare the `my_obj` variable as private or write a getter function for that object. We will do the latter:

```cpp
...
class scopedPtr {
...
	entity* getObject() { return m_object; }
}

int main(){
	scopedPtr e = new Entity();
	//we still can't do this:
	//e->print();
	e.getObject()->print();
}
```

This looks quite messy. We can change that by overloading the arrow operator:

```cpp
...
class scopedPtr {
...
	entity* operator-> () {
		return m_obj;
	}
};

int main() {
	scopedPtr e = new entity();
	e->print();
}
```

Now our arrow operator works the way we wanted. In a case where we want to create a const type scoped pointer, we can create a const type overload:

```cpp
...
class scopedPtr {
...
	entity* operator-> () {
		return m_obj;
	}
	const entity* operator-> () const {
		return m_obj;
	}
};

int main() {
	scopedPtr e = new entity();
	e->print();

	const scopedPtr eConst = new entity();
	eConst->print();
}
```

## Using The Arrow Operator To Get The Offset Of A Member Variable In Memory

```cpp
struct vector3
{
	float x, y, z;
};
```

Suppose we wanted to know the offset of the `y` variable was from the `vector3` instance's memory address. In reality we know that a float is 4 bytes so if the memory address of `x` was 0, the memory address of `y` would be 4. But if we were to move the variables around and it looked like this:

```cpp
struct vector3
{
	float x, z, y;
};
```

The struct is still the same and would function the same. But `y`'s address would become 8 instead of 4.

Here we will write something for ourselves that will tell us the offset of each member variable from each other. We can do it roughly like this:

```cpp
struct vector3
{
	float x, y, z;
};

int main(){
	int offset = (int)&((vector3*)nullptr)->x;
	//this would function the same just wanted to include it
	//int offset = (int)&((vector3*)0)->x;

	std::cout << offset << std::endl;
}
```

The output would be:

```
0
```

We can also do it for `y`:

```cpp
struct vector3
{
	float x, y, z;
};

int main(){
	int offset = (int)&((vector3*)nullptr)->y;
	//this would function the same just wanted to include it
	//int offset = (int)&((vector3*)0)->x;

	std::cout << offset << std::endl;
}
```

The output would be:

```
4
```

And the output for `z` would be 8. You can use this when you are trying to serialize data and want to figure out the offsets of certain things...

This solution is great because it doesn't instantiate a `vector3` object. We can see it more clearly if we changed the code to output when the constructor gets called:

```cpp
struct vector3
{
	float x, y, z;
	vector3() {
		std::cout << "created" << std::endl;
	}
};

int main() {
	int offset = (int)&((vector3*)nullptr)->x;
	
	std::cout << offset << std::endl;
}
```

If we run this code:

```
0
```

As you can see, we have not created an instance. This way of doing something like this is kind of ugly. 

You could also create functions to return the offset of certain members. But you would need to instantiate at least one object from this struct to use it:

```cpp
#include <iostream>
#include <cmath>

struct vector3
{
	float x, y, z;
	vector3() {
		std::cout << "created" << std::endl;
	}
	int getOffset(vector3& v, float& variable) {
		return std::abs((int)(&(v)) - (int)(&(variable)));
	}
};

int main() {
	vector3 v;
	std::cout << v.getOffset(v, v.y) << std::endl;
}
```

This would return:

```
created
4
```



