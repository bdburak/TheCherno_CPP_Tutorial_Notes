Here, we will go into more depth about [strings](Strings.md). String literals are a series of characters in-between two double quotes. 

```cpp
"Hello World!"
```

This is a string literal. By default string literals are cast as `const char` arrays.

String literals are stored in a read-only section of memory. That's why they have to be declared as `const`.

If you want to be able to change characters in a string literal that's inside a variable, you have to define it not as a pointer but as an array.

```cpp
//not declaring it as const might be an issue on some complilers
char* name1[] = "bob";
name1[0] = 'd';

char name2[] = "bob";
name2[0] = 'd';
```

Only `name2`'s "bob" will become "dob". `name1` will either give an error or it will not change, depending on your compiler.

## Interesting Types Of Characters

### Wide Character

```cpp
//you have to have the L before your string literal
const wchar_t* name = L"bob";
```

### 16 and 32 Bit Char
C++ 11 introduced this.

```cpp
const char16_t name1 = u"bob";
const char32_t name2 = U"bob";
```

## New Things To Do With Strings

C++ 14 brought these:

```cpp
#include <iostream>
#include <string>
using namespace std::stringLiterals;
```

### Appending To Strings

```cpp

int main(){
	//This doesn't work
	std::string hello = "Hello " + "world";
	
	//The s after the string literal makes it into a standard string 
	std::string hello = "Hello "s + "world";
}
```

### Multi-line Strings
You can have multi-line strings. This is useful if you want to write paragraphs of text or any kind of structured text, including tabs. The `R"()"` gets rid of the escape characters for special characters such as tabs or new lines and puts them directly as characters. 

```cpp
//You need to have it formated like this: R"(...)"
 std::string stringExample = R"(Line1
Line2
Line3
...)"; 

std::cout << stringExample << std::endl;

//This also works for char arrays
const char* charExample = R"(Line1
Line2
Line3
...)"; 

```

The output would be:

```
Line1
Line2
Line3
...
```