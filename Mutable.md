Mutable has 2 fairly different uses; use with [consts](Consts.md) and with [lambda](Lambdas.md)'s. It refers to something that ***can*** change.

## Mutable With Consts


```cpp
#include <iostream>
#include <string>

class entity {
private:
	std::string m_Name;
public:
	//returns a const string referance
	const std::string& getName() const{
		return m_name;
	}
};

int main(){
	const entity e;
	//if we didn't make the function itselft const, we wouldn't be able to call this function. It would give an error.
	e.getName();
}
```

If the `getName()` function had to modify a class member that didn't have anything to do with being const or being returned. Such as a counter for the `getName()` function to see how many times it was called. We would need to use the `mutable` keyword before that variable's declaration.

```cpp
#include <iostream>
#include <string>

class entity {
private:
	std::string m_Name;
	int m_debugCount = 0;
public:
	//returns a const string referance
	const std::string& getName() const{
		//this wouldn't work because it wasn't declared as mutable
		m_debugCount++;
		return m_name;
	}
};

int main(){
	const entity e;
	//if we didn't make the function itselft const, we wouldn't be able to call this function. It would give an error.
	e.getName();
}
```

This wouldn't work because `m_debugCount` wasn't declared as mutable.

We can change that like this:

```cpp
#include <iostream>
#include <string>

class entity {
private:
	std::string m_Name;
	mutable int m_debugCount = 0;
public:
	//returns a const string referance
	const std::string& getName() const{
		//this now would work
		m_debugCount++;
		return m_name;
	}
};

int main(){
	const entity e;
	//if we didn't make the function itselft const, we wouldn't be able to call this function. It would give an error.
	e.getName();
}
```

Now it would work.

## Using Mutable With Lambda's
We will not get into what [lambda](Lambdas.md)'s are. They are just little throw-away functions that we assign to variables. It works similar to JavaScript's anonymous functions. We will only show you how mutable is used with them:

```cpp
int main(){
	int x = 8
	auto f = [](){
		std::cout << "Hello" << std::endl;
	}
	f();
}
```

This is fine and if we want to send `x` as a value we also can do it:

```cpp
int main(){
	int x = 8
	// we have to have a capture method which can be; x, &, = ...
	auto f = [=](){
		//this would give an error because x is passed by function
		x++;
		//this would work fine
		std::cout << x << std::endl;
	}
	f();
}
```

But if we try to pass `x` by value( = or just as x), we wont be able to modify it. We can remedy that by making the lambda function mutable.

```cpp
int main(){
	int x = 8
	// we have to have a capture method which can be; x, &, = ...
	auto f = [=]() mutable{
		//now that the function is mutable, this would work
		x++;
		std::cout << x << std::endl;
	}
	f();
}
```

Now this won't actually change the actual function of `x`, but it will technically do this:

```cpp
int main(){
	int x = 8
	// we have to have a capture method which can be; x, &, = ...
	auto f = [=]() mutable{
		//it copied the x variable to y
		int y = x;
		//modifying y and not x
		y++;
		std::cout << y << std::endl;
	}
	f();
	std::cout << x << std::endl;
}
```

The output would be:

```
9
8
```

as you can see, x hasn't actually changed.

Generally speaking, `mutable` is mostly used for class functions and not lambdas.