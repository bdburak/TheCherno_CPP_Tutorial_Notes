Header files are traditionally used to declare functions so that they can be used in your projects. In a cpp file, we can use functions from any other cpp file, but we need to declare them first. If you only have one function to declare, then it might not make sense to use a header file. But if you have lots of functions in each of the external cpp files, then you should create a header file for each cpp file. This allows us to only change the header files if we need to change the parameters or the name of a function, instead of changing all the declarations in each file that we use the external functions.

>We need a common place to just store the declarations, not the functions(definitions) themselves.

  ## Creating and Using Header Files
  Let's say we have a logger function in `Logger.cpp`:
  >`Logger.cpp`
```cpp
#include <iostream>

void Log(const char* message) {
    std::cout << message << std::endl;
}
void Log(const int message) {
    std::cout << message << std::endl;
}
```
and we want to access this function in our main program. Then we can create a header file called `Logger.cpp` and just add the declarations of our functions.
>`Logger.h`
```cpp
#pragma once
void Log(const char* message);
void Log(const int message);
```
After we created the `Logger.h` header file, we can just `include` it in our main program and use the functions.
>`Main.cpp`
```cpp
#include "Logger.h"

int main(){
	Log("hello world");
	log(10);
}
```

## `#pragma once` Header Guard Use

The `#pragma once` preprocessor statement makes the compiler only include this file once. **It makes sure that the header file will only be included once in a single translation unit**. `#pragma` is a header guard. 
There is an example of how we might accidentally define something more than once and how the `#pragma once` can save us.

>`Log.h`
```cpp
...
void LogFunction(...);
struct Log {};
```
>`Common.h`
```cpp
...
void CommonFunction(...);
struct Log {};
```
>`main.cpp`
```cpp
#include "Log.h"
#include "Common.h"

int main(){
...
}
```

If we run this program, we will encounter a compiler error which says: `error C2011: 'Log': 'struct' type redefinition...`. We have accidentally redefined the same `Log` struct twice. If we had included the `#pragma once` preprocessor (header guard), we could have avoided this issue.

>`Log.h`
```cpp
#pragma once
...
void LogFunction(...);
struct Log {};
```
>`Common.h`
```cpp
#pragma once
...
void CommonFunction(...);
struct Log {};
```
>`main.cpp`
```cpp
#include "Log.h"
#include "Common.h"

int main(){
...
}
```
This program would have no issues running.

## `#ifndef` (Include Guard) The Traditional Way Of Avoiding Redefinitions

Instead of using `#pragma once` you can use `#ifndef _HEADERNAME_H` . This will make sure that the definitions are only included if the `_HEADERNAME_H` symbol isn't already present. An example:

>`Log.h`
```cpp
#ifndef _LOG_H
#define _LOG_H
void LogFunction(...);
struct Log{};

#endif
```

>! The symbol can be anything you like, but using the header file's name is more traditional.

The gist of it is like this:

>We start with the if defined include guard and give it a token to check.
```cpp
#ifndef <TOKEN>
```

> We define the token.
```cpp
#define <TOKEN>
```

> We write the declarations inside the `#ifndef`.
```cpp
void function1();
...
...
void funtionN();
```

> After we are finished with our declarations we end it with:
```cpp
#endif
```

If this header gets included multiple times in the same translation unit, it will check if the definition is defined and if it is, it will not include it.

## Header Guides

In some examples like the `#include <iostream>` include statement we use `<>` and in our header file include statements we use `""`. This might confuse many programmers. The explanation for the discrepancy is:

- `<>` is used for external files like external libraries. They are used with compiler include files.
- `""` is used for internal files like header files we wrote. They are used for relative directories like the same directory or any upper directories via `../` . 

## The Reason Why Some Include Statements For Libraries Don't Have File Extensions
The cpp standard library author decided that c and cpp standard libraries need to be distinguished. So the c standard libraries get file extensions and cpp standard libraries don't. 

>C standard library include example:
```cpp
#include <stdlib.h>
```

>C++ standard library include example:
```cpp
#include <iostream>
```
