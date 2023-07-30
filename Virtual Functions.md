Virtual functions allow us to override functions in sub-classes.

```cpp
#include <iostream>
#include <string>

class entity {
public:
	std::string GetName() { return "entity"; }
};

class player : public entity {
private:
	std::string m_name;

public:
	player(const std::string& name) {
		m_name = name;
	}
	std::string GetName() { return m_name; }
};

int main() {
	entity* e = new entity;
	std::cout << e-> GetName() << std::endl;

	player* p = new player("burak");
	std::cout << p->GetName() << std::endl;
}
```

This would work fine and would output:

```
entity
burak
```

But if we want to use polymorphism, i.e. passing a child object to a parent object, we would have problems.

```cpp
//...same classes...

int main(){
	//...same declerations...
	entity* eTop = p;
	std::cout << eTop-> GetName() << std::endl;
}
```

The output would be:

```
entity
burak
entity
```

But we don't want that to happen. We were expecting "burak" to be written but we got "entity". The same would happen if we changed our code to be:

```cpp
//...same classes...
void PrintName(entity* e){
	std::cout << e-> GetName() << std::endl;
}

int main(){
	//...same declerations...
	entity* eTop = p;
	PrintName(eTop);
}
```

The output:

```
entity
burak
entity
```

This happens because when we call a class function, the function that belongs to the class will be called. And we are inheriting the `GetName()` function from our `entity` class, so the program will call the original function. 

To resolve this we use `virtual functions`. Virtual functions are what are called `dynamic dispatch`, which compilers implement via a `vtable`.

>A `vtable` is basically a table which contains the mapping for all of the virtual functions inside our base class so that we can actually map them to the correct overridden function in runtime.

We can create a function as a virtual function with the keyword `virtual`. If we rewrite our program using virtual functions:

```cpp
#include <iostream>
#include <string>

class entity {
public:
	//we added virtual to the beginning
	virtual std::string GetName() { return "entity"; }
};

class player : public entity {
private:
	std::string m_name;

public:
	player(const std::string& name) {
		m_name = name;
	}
	std::string GetName() { return m_name; }
};

void PrintName(entity* e){
	std::cout << e-> GetName() << std::endl;
}

int main() {
	entity* e = new entity;
	std::cout << e-> GetName() << std::endl;

	player* p = new player("burak");
	std::cout << p->GetName() << std::endl;

	entity* eTop = p;
	PrintName(eTop);
}
```

This would give us the result that we wanted which is:

```
entity
burak
burak
```


We can also tag our overridden function in `player` with the keyword `override`. It was introduced in C++ version 11.

```cpp
class player: public entity {
	//...
	std::string GetName() override { return m_name; }
};
```
> It isn't required, but it makes your code a little bit more readable and helps us avoid spelling mistakes like this:

```cpp
class player: public entity {
	//...
	std::string getName() override { return m_name; }
};
```

This code would give us an error.


## The Cost Of Virtual Functions

There are 2 big performance costs associated with virtual functions:
- The additional memory that we have to use for vtables. Which is a memory cost.
- Every time we have to go through that table to get to the function that was intended. Which is a performance cost.

Because of these costs some people prefer not to use virtual functions at all. In reality though, these costs don't really effect our program significantly.

> Embedded platforms may be effected more significantly.