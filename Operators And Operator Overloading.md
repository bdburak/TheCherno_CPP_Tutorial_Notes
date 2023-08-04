We can used operator overloading to override the functionality of existing operators such as; +, -, >, new, delete, ',' , '()'…
It's a good thing that C++ lets us modify basically everything, but it might become a double edged sword if you are not careful.
Operators are just functions. Instead of calling it `add()`, they created an operator called `+`. 

>You should refrain from overloading operators senselessly. It will cause other developers, and your future self, to have a hard time understanding your code. 

Lets create an example:

If we don't want to use overrides we would do it like this:
```cpp
#include <iostream>

struct vector2 {
	float x, y;

	vector2(float x, float y)
		:x(x), y(y)	{}

	vector2 add(const vector2& other) const {
		return vector2(x + other.x, y + other.y);
	}
};

int main() { 
	vector2 position(4.0f, 4.0f);
	vector2 speed(0.5f, 1.5f);

	vector2 result = position.add(speed);

}
```

Where we have created a function that adds the passed in vector2's values to a new vector2 with the native vector2 values of the entity.

It would be fine if it was just this, but we might want to multiply the vector by some amount later and that would take another function.

```cpp
#include <iostream>

struct vector2 {
	float x, y;

	vector2(float x, float y)
		:x(x), y(y)	{}

	vector2 add(const vector2& other) const {
		return vector2(x + other.x, y + other.y);
	}
	vector2 multiply(const vector2& other) const {
		return vector2(x * other.x, y * other.y);
	}
};

int main() { 
	vector2 position(4.0f, 4.0f);
	vector2 speed(0.5f, 1.5f);
	vector2 multiplier(1.1f, 1.1f);
	
	vector2 result = position.add(speed.multiply(multiplier));

}
```

This code is quite messy. Now we will take advantage of operator overloading and make it cleaner:

```cpp
#include <iostream>

struct vector2 {
	float x, y;

	vector2(float x, float y)
		:x(x), y(y) {}

	vector2 add(const vector2& other) const {
		return vector2(x + other.x, y + other.y);
	}

	vector2 operator+(const vector2& other) const{
		return add(other);		
	}

	vector2 multiply(const vector2& other) const {
		return vector2(x * other.x, y * other.y);
	}
	vector2 operator*(const vector2& other) const {
		return multiply(other);
	}
};

int main() { 
	vector2 position(4.0f, 4.0f);
	vector2 speed(0.5f, 1.5f);
	vector2 multiplier(1.1f, 1.1f);
	
	vector2 result = position + speed * multiplier;
}

``` 

We have overloaded the `+` and `*` operators to work with our vector2 objects. 

You can also do something weird and call the + operator overload from the add function:

```cpp
...
vector2 add(const vector2& other) const {
	return *this + other;
}

vector2 operator+(const vector2& other) const{
	return vector2(x + other.x, y + other.y);	
}
...
```

The same thing can be done like this where we use the operator overload as a function:

```cpp
...
vector2 add(const vector2& other) const {
	return operator+(other);
}

vector2 operator+(const vector2& other) const{
	return vector2(x + other.x, y + other.y);	
}
...
```

We can also overload the `<<` operator so that we can print our vector more easily:

```cpp
#include <iostream>

struct vector2 {
	float x, y;

	vector2(float x, float y)
		:x(x), y(y) {}

	vector2 add(const vector2& other) const {
		return vector2(x + other.x, y + other.y);
	}

	vector2 operator+(const vector2& other) const{
		return add(other);		
	}

	vector2 multiply(const vector2& other) const {
		return vector2(x * other.x, y * other.y);
	}
	vector2 operator*(const vector2& other) const {
		return multiply(other);
	}
};

//the ostream parameter would be std::cout
std::ostream& operator<<(std::ostream& stream, const vector2& other){
	stream << other.x << ", " << other.y;
	return stream;
}

int main() { 
	vector2 position(4.0f, 4.0f);
	vector2 speed(0.5f, 1.5f);
	vector2 multiplier(1.1f, 1.1f);
	
	vector2 result = position + speed * multiplier;

	std::cout << result << std::endl;
}
```

When we run this code, it will output:

```
4.55, 5.65
```

It did it without a problem.

We can also overload the == and != operator to compare two same class entities:

```cpp
struct vector2 {
...
	bool operator==(const Vector2& other) const{
		return x == other.x && y == other.y;
	}
	bool operator!=(const Vector2& other) const{
		// we had already overloaded the == operator so why not use it
		return ((*this*) == other);
	}
};
...

int main(){
	vector2 a(2.0f, 1,2f);
	vector2 b(2.0f, 1,2f);
	if(a == b)
		std::cout << "a == b" << std::endl
	if(a != b)	
		std::cout << "a != b" << std::endl
}
```

This would output:

```
"a == b"
```


## How to Create An Operator Overload
The general layout is:

```cpp
struct className{
	className operator+(const className& c) const{
		//do something
	}	
}
```

You can do this for all operators. The const's are not necessary, but you should use them if you are not modifying class members in that function. We will overload the actual function of the operator and not inside our class:
