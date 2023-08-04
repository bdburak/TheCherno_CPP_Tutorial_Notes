Smart pointers are a way to automate the `new` and `delete` processes. With smart pointers, you don't have to call delete after you create with new. In fact, in many cases with smart pointers, you don't even have to call new either.
Smart pointers are a wrapper around a raw pointer. Let's create a simple smart pointer:

## Unique Pointer(Scoped Pointer)
It's a pointer where it will delete itself when it gets out of scope. We have talked about them [here](obsidian://open?vault=c%2B%2B&file=Object%20Lifetime). They are called unique pointers because they have to be unique. You can't copy them. If you copy a unique pointer, they both will point to the same address. If one of them gets out of scope, then it will delete itself and the copy of it will point to nothing.

In order to have access to smart pointers you need to include the memory library using `#include <memory>`.

```cpp
#include <iostream>
#include <string>
#include <memory>

class entity {
public:

	entity() {
		std::cout << "created entity!" << std::endl;
	}
	~entity() {
		std::cout << "destroyed entity!" << std::endl;
	}
	void print() {
		std::cout << "entity printed" << std::endl;
	}
};

int main() {
	{
		//std::unique_ptr<entity> e(new entity());
		//this isn't allowed because the constructor for unique_ptr is an explicit constructor
		//std::unique_ptr<entity> e = new entity();

		

		//the prefered way of creating unique pointers:
		std::unique_ptr<entity> e = std::make_unique<entity>();
	//accessing 
		e->print();
	}
	std::cout << "Hello world" << std::endl;
}
```

The output would be:

```
created entity!
entity printed
destroyed entity!
Hello world
```

As you can see, the smart pointer deleted itself after it got out of scope, just like a stack allocated variable. The difference is that the `entity` object that was created and pointed to was allocated in the heap and was also deleted after the smart pointer got out of scope.

If you try to copy the unique pointer, you will get an error:

```cpp
...
int main(){
	{
		std::unique_ptr<entity> e(new entity());
		//this will give an error
		std::unique_ptr<entity> e0 = e;
	}
	
}
```

### Unique Pointer Creation
Simplest way to create is:
`std::unique_ptr<className> objectName(new className());`

But we prefer to create unique pointers like this:
`std::unique_ptr<className> objectName = std::make_unique<className>();`
This is for exception handling. If there's a problem with your constructor your pointer wont be dangling in the air.

## Shared Pointers
The way a shared pointer is implemented is really up to your compiler. However in most cases it uses a system called "[reference counting](https://mortoray.com/what-is-reference-counting/)". Reference counting is a practice where you keep track of how many references you have to your pointer. When that counter reaches zero, that's when it gets deleted.
If we create a smart pointer and create a copy of it. The memory address they point to wont be freed before both pointers gets deleted.

Using a shared pointer:

```cpp
...class entity..
int main() {

	{
		std::shared_ptr<entity> sharedE0 = std::make_shared<entity>();
		//This is allowed with shared pointers
		std::shared_ptr<entity> sharedE1 = sharedE0;
	}
	
	std::cout << "Hello world" << std::endl;
}
```

You could also do it like this but it isn't advised with shared pointers:
`std::shared_ptr<entity> sharedE(new entity);`
Shared pointer has to allocate another block of memory called a "control block" where 
where it stores that control block if you first create an entity and then, pass it into the shared pointer constructor there will have to be 2 allocations instead of 1. If you use the s`td::make_shared<className>()`, it will do them both at the same time.

We can also demonstrate the reference counting behavior with this:

```cpp
int main() {

	{
		std::shared_ptr<entity> sharedE0;		
		{
			std::shared_ptr<entity> sharedE1 = std::make_shared<entity>();
			sharedE0 = sharedE1;
		}
	}
	
	std::cout << "Hello world" << std::endl;
}
```

The `sharedE1` will get out of scope before `sharedE1`. This would normally cause problems with unique pointers but if we run this code line by line, we will see that `sharedE1`'s destructor doesn't get called before `sharedE0` gets out of scope. The output for this code would be:

```
created entity!
destroyed entity!
Hello world
```

But "destroyed entity!" wasn't printed before `sharedE0`'s scope ended. 

### Weak Pointer
This is a pointer you can use with shared pointers. You can assign shared pointers to it. When you assign a  weak pointer to a shared pointer, it doesn't increase the reference counter. This can be used if you don't want to take ownership of the entity. If you want to store a list of entities and you don't even care about the validity of them. You just want to store a reference of them.

Using weak pointers:

```cpp
int main() {

	{
		std::shared_ptr<entity> sharedE0 = std::make_shared<entity>();
		
		//created a weak pointer
		std::weak_ptr<entity> weakE = sharedE0;
	}
	
	std::cout << "Hello world" << std::endl;
}
```

We can also see how it would work in the previous example with 2 pointers that have different scopes:

```cpp
int main() {

	{
		std::weak_ptr<entity> weakE; 
		{
			std::shared_ptr<entity> sharedE = std::make_shared<entity>();
			weakE = sharedE;
		}
	}
	
	std::cout << "Hello world" << std::endl; 
}

```

Here, we can observe that the entity that was created by our shared pointer, will be deleted after the inside scope ended. This is different from before, where the outer shared pointer made it wait before deleting itself. 

## When Should You Use Them
You should use them whenever you can. They automate your memory management, they prevent you from accidentally leaking memory by forgetting to delete things. They are quiet useful.
The shared pointer has some overhead that you should keep in mind.
Smart pointers don't replace `new` and `delete`. You should still use them if you want to.

The preference of using smart pointers should be:
unique pointers -> shared pointers
