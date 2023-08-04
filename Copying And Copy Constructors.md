Understanding how copying works in C++ is really important. If you copy unnecessarily, it can lead to performance issues. Also if you don't want something to copy and it copies something anyways. This is something that we want to avoid. 

## Copying 
The simplest way of copying is primitive variable copying:

```cpp
int a = 2;

int b = a;
```

Here, `b` only copied the value of `a`. If we change `a`, `b` wouldn't be effected.

The same would apply for structs and classes:

```cpp
struct vec2{
	float x,y;
};
vec2 a = {2, 3};

vec2 b = a;
```

`b` would only be a copy of `a`'s value and wouldn't be connected. We can modify both of them and they wouldn't be effected by each other.

The same couldn't be said about pointers and heap allocated variables or objects:

```cpp
struct vec2{
	float x,y;
};

vec2* a = new vec2();
vec2*b = a;
```

If we were to modify or delete `a`, `b` would be effected. We have basically copied the pointer and not the value. If we were to do this:

```cpp
struct vec2{
	float x,y;
};

vec2* a = new vec2();
vec2*b = a;

b->x = 2.0f;
```

This would change `a`'s `x` value.

When you are just assigning a variable or object to another one, you are just copying it. The only exception being [references](References.md). This isn't the case for pointers and therefore heap allocated things.

## Example
Here we will write a simple string class where we will use `memcpy()` to copy a buffer to another:

```cpp
#include <iostream>

class String {
private:
	char* m_buffer;
	unsigned int m_size;
public:
	String(const char* string) {
		m_size = strlen(string);
		//including the null termination character
		m_buffer = new char[m_size+1];
		memcpy(m_buffer, string, m_size);
		//we don't know if the char array will have a null terminator so we can add one
		m_buffer[m_size] = 0;
	}
	//this allows us to use private class members inside this function
	friend std::ostream& operator<<(std::ostream& stream, const String& string);
};

std::ostream& operator<<(std::ostream& stream, const String& string) {
	stream << string.m_buffer;
	return stream;
}

int main() {
	String string = "burak";
	std::cout << string << std::endl;
}
```

In the code above we have a memory leak because we never delete the string object. We don't even have a destructor function that can properly do it.

```cpp
#include <iostream>

class String {
...
~String (){
	delete[] m_buffer;
}
...

int main() {
	String string = "burak";
	std::cout << string << std::endl;
}
```

Let's try to copy a the string object to another instance:

```cpp
...
int main() {
	String string = "burak";
	String second = string;
	std::cout << string << std::endl;
}
```

We will encounter an error after the program finishes. It is caused by trying to delete the contents of a memory address that was already deleted. This happens because when we copy a `String` object, the `m_buffer` pointer only gets copied. This means it points to the same address for both objects. When we copied the string, we only did a shallow copy.

We can also observe that they occupy the same memory address if we change an index of the copy. The change will effect both the original and the copy. Of course, we will have to write an [operator overload](obsidian://open?vault=c%2B%2B&file=Operators%20And%20Operator%20Overloading) for `[]` to be able to change an index.

```cpp
#include <iostream>

class String {
...
	char& operator[](unsigned int index) {
		return m_buffer[index];
	}
...
};
...
int main() {
	String string = "burak";
	String second = string;
	second[1] = 'i';
	std::cout << string << std::endl;
	std::cout << second << std::endl;
}
```

Here we can observe the console print out:

```
birak
birak
```

Both the original and the copy have changed. The error hasn't been remedied yet.

In order to fix the error, we have to copy the contents of the `m_buffer` to another location and point the copied `m_buffer` to that location. The `m_size` wont pose an issue due to it being a primitive type and the contents of the address copied to the copy. We want the copied string to be its own string and not be effected or effect the original string. The way we can do that, is a technique called a [deep copy](https://www.geeksforgeeks.org/shallow-copy-and-deep-copy-in-c/). In order to do that, we will need a copy constructor.

## Copy Constructors
The copy constructor gets called when an object is getting copied, when you assign it to another object via `=`. C++, by default, will provide you wit a copy constructor. Here we can see the anatomy of a copy constructor:

`className(const className& other){}`

The one supplied by default will only do a simple `memcpy` operation where the memory is copied i.e. a shallow copy.

If we were to write the default copy constructor for our `String` class:

```cpp
class String{
...
	String(const String& other)
		:m_buffer(other.m_buffer), m_size(other.m_size)
	{
	}
...
};
```

Or if you want to be more exciting:

```cpp
class String{
...
	String(const String& other){
		memcpy(this, &other, sizeof(String));
	}
...
};
```

Well this won't cut it for our `String` class. We want to do a deep copy. So we need to override the default copy constructor and write our own.

```cpp
class String{
...
	//we can shallow copy the size because it's an integer
	String(const String& other)
		: m_size(other.m_size)
	{
		m_buffer = new char[m_size + 1];
		//we don't have to add our own null termination character because a String has to have one by default(we coded that in)
		memcpy(m_buffer, other.m_buffer, m_size + 1);
	}
...
};
```

Now we can show the entire code:

```cpp
#include <iostream>

class String {
private:
	char* m_buffer;
	unsigned int m_size;
public:
	String(const char* string) {
		m_size = strlen(string);
		//including the null termination character
		m_buffer = new char[m_size+1];
		memcpy(m_buffer, string, m_size);
		//we don't know if the char array will have a null terminator so we can add one
		m_buffer[m_size] = 0;
	}
	~String() {
		delete[] m_buffer;
	}
	//we can shallow copy the size because it's an integer
	String(const String& other)
		: m_size(other.m_size)
	{
		m_buffer = new char[m_size + 1];
		//we don't have to add our own null termination character because a String has to have one by default(we coded that in)
		memcpy(m_buffer, other.m_buffer, m_size + 1);
	}

	char& operator[](unsigned int index) {
		return m_buffer[index];
	}

	//this allows us to use private class members inside this function
	friend std::ostream& operator<<(std::ostream& stream, const String& string);
};

std::ostream& operator<<(std::ostream& stream, const String& string) {
	stream << string.m_buffer;
	return stream;
}

int main() {
	String string = "burak";
	String second = string;
	second[1] = 'i';
	std::cout << string << std::endl;
	std::cout << second << std::endl;
	int a;
}
```

And if we run this, the result would be:

```
burak
birak
```

And we wouldn't get an error like we did before.

## Passing Objects As Reference Instead Of Copying Them
If we were to write an external print function for our `String` type objects, we can either accept them as copies or references to the real object. A copy type example would be:

```cpp
class String(){
...
String(const String& other)
		: m_size(other.m_size)
	{
		...
		std::cout << "copied" << std::endl;
	}
...
};
...
void printString(String string) {
	std::cout << string << std::endl;
}
int main() {
	String string = "burak";
	printString(string);
}
```

The output would be right but the `string` object would need to be copied and new allocations to the heap would happen. This is quite inefficient. 
In the output of this program, we can clearly see that the string object was copied before printing it:

```
copied
burak
```

We can avoid this by just passing it as a reference:

```cpp
class String(){
...
String(const String& other)
		: m_size(other.m_size)
	{
		...
		std::cout << "copied" << std::endl;
	}
...
};
...
void printString(const String& string) {
	std::cout << string << std::endl;
}
int main() {
	String string = "burak";
	printString(string);
}
```

The output would become:

```
burak
```

When we accept it as a const reference, we guarantee that we are not going to modify the original string.

>In general, always pass your objects with a const reference

### Disabling Copying
You can disable copying your object by just disable the copy constructor by overriding it.

```cpp
class className{
	...
	className(const className& other) = delete;
};
```

By the way, this is exactly what [unique pointer](obsidian://open?vault=c%2B%2B&file=Smart%20Pointers) does.