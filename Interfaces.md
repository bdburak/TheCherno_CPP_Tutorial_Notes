Interfaces are pure [[Virtual Functions]] in C++. A pure virtual function allows us to define a in a base class that does not have an implementation, it doesn't contain the function declaration i.e. the content of a function, and then for subclasses to actually implement that function.

> C++ doesn't have an interface keyword, unlike other languages. So we have to use classes as interfaces.

If we look at our example from [[Virtual Functions]], we had a virtual `GetName()` function in the `entity` class. And we had an override in the `player` class for that function.

By having the body of the function in the base class, we alleviate the need to create an override in the sub-classes.

However, in some cases it doesn't make sense for us to provide a default implementation for a function. We might actually want to force the sub-class to provide its own definition for a certain function. 

It is quite common for us to create a class that only consists of unimplemented functions for sub-classes to implement them. This is often referred to as an **interface**.

>Interfaces are, classes of unimplemented methods acting as templates. 

>Because interfaces consist of unimplemented methods, we can't even instantiate objects from them.

## Example

We will rewrite the `GetName()` function from the `entity` class from [[Virtual Functions]] and [[Inheritance]] to be a pure virtual function:

```cpp
class entity {
public:
	//the =0 makes it a pure virtual function
	virtual std::string GetName() = 0;
}
```

The `= 0` makes the function un-instantiable and forces the sub-classes to implement it themselves.

In the `main()` function we can see that the `entity` instance will give us an error:

```cpp
int main() {
	...
	//              V this would have red squigly lines under it
	entity* e = new entity(); 
}
```

We could solve this easily by creating the `entity` instance through a class that ***has*** actually implemented the `GetName()` virtual function.

```cpp
int main() {
	...
	//              V this would have red squigly lines under it
	entity* e = new player(""); 
}
```

This would work fine because we implemented the `GetName()` function via overriding it in the `player` class like so:

```cpp
class player : public entity {
	...
public:
	...
	// We have overridden and implemented the function, so we can use it now.
	std::string GetName() override { return m_name; }
}
```

Without implementing it in `player` as well, we wouldn't be able to instantiate an `entity` instance with a `player` class either. 

## Example 2

We want to create a function that takes in a parameter and returns the type of class. To do that in our previous code:

```cpp
void printType(???* obj) {
	std::cout << obj->GetClassName() << std::endl;
}

//change it into:
void printType(printable* obj) {
	std::cout << obj->GetClassName() << std::endl;
}
```

Something like this. For this to work, we need a type that guarantees that we actually have a `GetClassName()` function. We need a type that provides that `GetClassName()` function. That's exactly what an interface is.

We will create an interface called `printable` that will have a pure virtual function called `GetClassName()`:

```cpp
class printable {
public:
	virtual std::string GetClassName() = 0;
}
```

Now we will inherit that interface into the `entity` class, which will pass it down to any sub-class.

```cpp
class entity : public printable {
public: 
	std::string GetName() override { return "entity"; }

	//implementing the override function
	std::string GetClassName() override {return "entity";}
}
```

>However if it wasn't, for any reason, implemented for `player` class we can just inherit `printable` as well.

```cpp
class player : public entity, printable {
	...
}
```

Right now we can instantiate `entity`'s and `player`'s but we will run into an issue:

```cpp
int main(){
	entity* e = new entity();
	player* p = new player("burak");

	printType(e);
	printType(p);
}
```

This would result in this output:

```
entity
entity
```

We wanted "player" to be the last printed text but it printed "entity". This happened because we didn't override the `GetClassName()` function for the `player` class. Thanks to the `entity` class's implementation, we were able to instantiate a `player` object. But the `player` class directly inherits the `GetClassName()` function that the `entity` class implemented, which returns "entity".
If we want to avoid this, we have to implement it for every sub-class of the `entity` class:

```cpp
class player : public entity {
	...
	//implementing the override function
	std::string GetClassName() override {return "player";}
}
```

Now we will get the right result:

```
entity
player
```