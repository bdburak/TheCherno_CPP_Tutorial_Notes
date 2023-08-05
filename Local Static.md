We have explored [static](obsidian://open?vault=c%2B%2B&file=Static) before. In this tutorial we will explore another context for static, the local scope. You can use `static` in a local scope to declare things. This has a different meaning than the other two we have been through. This will make more sense if we were to consider what kind of considerations we have to make when declaring a variable; the scope and lifetime. Lifetime refers to how long it will remain in our memory before it gets deleted. The scope refers to where we can actually use that variable.

So a `static local variable` allows us to declare a variable, which has a lifetime of essentially our whole program. However, its scope will be determined by the function it is in. This wouldn't be limited to functions. This was just an example and any other scope would be the same. This shows that there is no difference between static in a function's scope and static in a class's scope. Because the lifetime will be the same. The only difference being, the class scope's members will also be able to access them.

Let's see some examples:

## Example

```cpp
#include <iostream>

void Function() {
	static int i = 0;
	i++;
	std::cout << i << std::endl;
}

int main() {
	Function();
	Function();
	Function();
	Function();
}
```

Declaring the `i` variable as `static` inside our funtion means that the first time the function is called, the `i` variable will be assigned 0. And any subsequent calls to the function will not create a new variable, but use the same variable and increment the value by 1. 

We can see this more clearly in the output:

```
1
2
3
4
```

If we declared `i` as a non-static variable, we would see this console output:

```
1
1
1
1
```

When we declared the `i` variable as static. It almost worked like a global variable. Just like this:

```cpp
int i = 0;
void Function() {
	i++;
	std::cout << i << std::endl;
}
```

The only difference being, `i` is only accessible by the `Function()` function.

## Uses Of Local Static
Some people discourage the use of local static. However, Cherno doesn't. You certainly can achieve basically the same functionality in other ways but, this can also be said for classes. You can write the same programs without using classes however, classes make our lives easier. In the same sense, we can use local static in our advantage to make our life easier as well.

Another use would be [singleton](https://en.wikipedia.org/wiki/Singleton_pattern) classes. A singleton class should only have 1 instance of itself. If we were to write a singleton class without the use of local static:

```cpp
#include <iostream>

class Singleton {
private:
	static Singleton* s_instance;
public:
	static Singleton& Get() { return *s_instance; }
	
	void PrintHello() { std::cout << "hello" << std::endl; }
};

Singleton* Singleton::s_instance = nullptr; 

int main() {
	Singleton::Get().PrintHello();
}
```

Here we have only one instance of `Singleton` and we can use that instance to do whatever we want.

An example of creating a singleton class ***with*** the use of local static could be:

```cpp
#include <iostream>

class Singleton {
public:
	static Singleton& Get() {
		static Singleton instance;
		return instance;
	}
	
	void PrintHello() { std::cout << "hello" << std::endl; }
};

int main() {
	Singleton::Get().PrintHello();
}
```

This will have the exact same behavior. But it is cleaner.

