Strings are a group of characters. 

## The C Way Of Creating Strings
In C, strings are created via `char` arrays. Like this:

```cpp
const char* stringName = "Hello World!";
```

> Even though these are declared with a char pointer, they are not heap allocated! You don't need to `delete` these like you would with a heap allocated array.

> The rule of thumb for the `delete` keyword is; ***don't `delete` them, unless you used `new` to create them!*** 

Because we created the string with a `const char*` pointer, we can't mutate any of the characters afterwards. So something like this wouldn't work:

```cpp
const char* stringName = "Hello World!";
stringName[1] = "o";
```

### How It Is Stored In Memory
`char` array strings are stored just like any other array in memory, this being side by side. You can see it in this example:

![[Pasted image 20230725021613.png]]

This shows how the "Hello World!" string was stored.

### How The Size Is Determined
When we try to output our string to the console, the program has to know how many bytes of characters it has to pull from memory. It is done using `0x00` at the end of the string in memory. In the example above, we can see that after `0x21` comes `0x00`. This indicates that the string is done.
>`0x00` is called the `null termination` character.

### Creating A String Manually
You can also create your `char` array yourself. You need to remember to put the `null termination` character at the end.

```cpp
//both of these would work
char name[4] = {'b', 'o', 'b', "\0"};
char name[4] = {'b', 'o', 'b', 0};
```

This works fine and will output "bob" to the console without a problem;

## C++ Way Of Creating Strings
C++ has a library called `string` that is part of the standard library. It is a templated version of the `basic_string` class. It is a template specialization of the `basic_string` class with `char` as the template parameter. Which means,  `char` is the underlying data type for each character.

### Using `std::string`
We will change the "Hello World!" char array to become a `std::string` object:

```cpp
#include <iostream>
#include <string>

int main(){
	std::string stringName = "Hello World!";
}
```

When you hover over the "Hello World!" string, you can see that it was passed to the string constructor as a `const char` array:
![[Pasted image 20230725023814.png]]
Of course a string variable is a full object. So, we get many functions that come with it.

### String Functions

#### Finding The Size Of A String
This will give us the ***character length***, the number of characters it has. Unlike a char array size, which will give you how many bytes it has. 

```cpp
std::string name = "Hello World!";
std::cout << name.size() << std::endl;
```

This will output:

```
12
```

Which is the number of characters it has.

#### Appending Strings
You unfortunately can't just use the `+` operator.  Like this:

```cpp
std::string name = "Hello " + "World!";
```

This wouldn't work. The reason for that is, you are trying to add 2 `const char` arrays together. There are a couple of things you could do:

```cpp
std::string name = "Hello ";
name += "World!";
```

This works because you are adding a pointer to a string. Strings have overloaded the `+` or `+=` operators to let you add strings in this way.

Or you could do:

```cpp
std::string name = std::string("Hello ") + "World!";
```
You are explicitly calling the string constructor to cast "Hello " as a string and basically do the same thing as above, but in the same line.

#### Finding Text In A String
You can use the `find()` function to get the starting position of a keyword or substring in your string:

```cpp
std::string name = "Hello World!";

//This will return the starting position of "lo"
std::cout << name.find("lo") << std::endl;
```

If you want to know if your string **contains** something:

```cpp
std::string name = "Hello World!";

//This will return a boolean value of true if it found it.
bool contains = name.find("lo") != std::string::npos;
```

The `std::string::npos` is a constant value, which is the highest value a [size_t](https://cplusplus.com/size_t) type can get. If the counter in the `find()` function gets to the max size and can't find your substring, it will return this value.

#### Where To Find All The Functions
You can head to [this](https://cplusplus.com/reference/string/string/) link to find all the functions for strings.

### Passing Strings Around Other Functions
What you shouldn't do:

```cpp
void PrintString(std::string string){
	std::cout << string << std::endl;
}
```

This is a wrong way of doing this. When you try to accept a `std::string` type variable into your function like this. You are creating a copy of that class and giving it to the function. In the example above, it wouldn't effect it but if you were trying to mutate the parameter inside your function, it wouldn't effect the real variable you passed in. 

```cpp
void AppendWorldToString(std::string string){
	string += " World!";
}

int main(){
	std::string name = "Hello";
	AppendWorldToString(name);
	std::cout << name << std::endl;
}
```

This would output:

```
Hello
```

It also would use extra memory to create a brand new string copy in the heap and string copying is really slow.

#### Passing A String Into A Read Only Function
A read only function wouldn't modify a variable, but only read it. The printing functions from earlier would be the perfect example for this:

```cpp
void PrintString(std::string string){
	std::cout << string << std::endl;
}
```

But we need to change the parameter to be a [const](Const.md) type [reference](References.md), so that our program will now that it wont be modified and shouldn't be copied:

```cpp
void PrintString(const std::string& string){
	std::cout << string << std::endl;
}
```

#### Passing A String Into A Non-Read Only Function
If you want to modify a string object that has been passed as a parameter, we need to take it in as a [pointer](Pointers.md). You can see how it is done in the [[Pointers]] document.

The `AppendWorldToString()` function from above is a good example that we can fix:

```cpp
void AppendWorldToString(std::string* string) {
	*string += " World!";
}

int main() {
	std::string name = "Hello";
	AppendWorldToString(&name);
	std::cout << name << std::endl;
}
```