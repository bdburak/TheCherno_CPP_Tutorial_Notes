We will learn how you should be creating your objects in C++.  We can create objects either on the heap or the stack portion of our memory. 
- When we create stack allocated objects they will have a limited lifespan, until it gets out of scope.
- When we create heap allocated objects, they stay on the heap until we either delete them from the heap or close the program.

## Creating Them In Practice

Our base [class](Classes.md): 

```cpp
#include <iostream>
#include <string>

using String = std::string;

class entity {
private:
	String m_name;
public:
	entity() : m_name("unknown") {}
	entity(const String& name) : m_name(name) {}

	const String& getName() const { return m_name; }
};
```

### Creating An Object On The Stack

```cpp
int main(){
	entity e0;
	entity e1 = entity("bob");
}
```

These `e0` and `e1` objects were created on the stack because we didn't use the `new` keyword to create them. they will have a limited lifespan and will be deleted after they get out of scope. 

If you can create objects like this, then you should create them on the stack whenever possible. It is the fastest and safest way of creating them. You don't have to manage them and you can let the computer manage the memory usage.

#### When Not To Create Objects On The Stack
If you want your object to be available after it was used. You want to create it on the heap. Because after `e0` and `e1` get used in the `main()` function, they will be deleted from memory. This would be an issue if you wanted to use them in other functions.

```cpp
void funtion(){
	entity e = entity("taylor");
}
```

As soon as we get to the ending curly bracket, the entity `e` will be deleted and become unavailable to use in other functions.

Another reason for not creating objects on the stack would be, because we have too many objects. The stack is actually quite small and if we try to create too many objects on the stack we wont be able to. The stack is usually only a couple of megabytes at most.

### Creating Objects On The Heap
We have to change the type of our object in order to create it on the heap. We have to change it to a [pointer](Pointers.md). We also have to create it with the [new](obsidian://open?vault=c%2B%2B&file=The%20New%20Keyword) keyword:

```cpp
int main(){
	entity* e0 = new entity();
	entity* e1 = new entity("bob");
}
```

When you are done with your heap allocated objects, you have to delete them like this:

```cpp
delete e0;
delete e1;
```

The `delete` keyword will delete our objects. If you have an array of objects you have to do it like this:

```cpp
delete[] objectArrayName;
```

In order to use the heap allocated objects, we can use 2 different methods:

```cpp
//dereference then, use it:
std::cout << (*e0).getName << std::endl;

//or just use the arrow operator that dereferences it for us
std::cout << e0->getName << std::endl;
```

You can see how the arrow operator works [here][obsidian://open?vault=c%2B%2B&file=The%20Arrow%20Operator].
### Heap Allocation Disadvantages
- Creating heap allocated objects takes more time than stack allocated ones.
- You have to manage it yourself i.e. deleting it after you no longer need it.

We can get the best of both worlds if we use [[Smart Pointers]].  We will get to them later.