`this` is only available for us through member functions. Inside those functions we can access `this`. `this` is a pointer to the current object instance that the method belongs to. Other than static functions, member functions have to be called though a class instance.

```cpp
class entity {
public:
	int x, y;

	entity(int x, int y) {
		x = x;
		y = y;
	}
};
```

Here we can see, we have the same variable names as the parameters and our compiler will just assign the parameter x and y to themselves if we do it like this.
We can avoid that by using the `this` keyword to specify that the x and y we cant to assign the parameter x and y values are the class variables.

```cpp
class entity {
public:
	int x, y;

	entity(int x, int y) {
		this->x = x;
		this->y = y;
	}
};
```

The `->` [arrow operator](obsidian://open?vault=c%2B%2B&file=The%20Arrow%20Operator)will de-reference our x and y member variables automatically. This is the equivalent to `(*this).x = x;`.

## Passing This To An Outside Function
We can also pass the current class instance using `this`. This is a pointer to our instance so we can just use it as such.

```cpp
void printEntity(entity* e);

class entity {
public:
	int x, y;

	entity(int x, int y) {
		this->x = x;
		this->y = y;
	}
	int getX() {
		printEntity(this);

		return this->x;
	}
};

void printEntity(entity* e) {
	std::cout << e->x << ", " << e->y << std::endl;
};
```

If we wanted to pass a const reference as a parameter in our outside function, we would only need to dereference our `this` pointer when passing it in.

```cpp
void printEntity(const entity& e);

class entity {
public:
	int x, y;

	entity(int x, int y) {
		this->x = x;
		this->y = y;
	}
	int getX() {
		printEntity(*this);

		return this->x;
	}
};

void printEntity(const entity& e) {
	std::cout << e->x << ", " << e->y << std::endl;
};
```

## Under The Hood
There isn't a whole lot that is going on under the hood when it comes to `this` keyword. But we should see how it works:

In a non-const function:

```cpp
class entity{
	...
	entity(...){
		//this is equivalent to:
		entity& e = *this;
		...
	}
};
```

In a const function:

```cpp
class entity{
	...
	getX(...) const{
		//this is equivalent to:
		const entity& e = *this;
		...
	}
};
```